+++
date = '2026-08-22T02:45:35+08:00'
draft = false
title = 'Kcalc Self Deadlock'
categories = ['Debugging']
tags = ['linux-kernel', 'c', 'concurrency']
+++

## Context

[kcalc](https://github.com/xDDoubleTea/kcalc) is a calculator running directly in kernelspace.
Achieved by loading as a Linux kernel module, and by exposing a character device `/dev/kcalc_chardev` to the userspace, supporting
`read, write` operations.

- A process `write` the expression they want to evaluate to `/dev/kcalc_chardev`.
- That process then `read` from `/dev/kcalc_chardev`.
- The character device is locked whenever a process is writing to/reading from it via `mutex_lock();`.
- The kernel is tested using `pytest`, calls `tee` and `cat` for `write` and `read` operations respectively

## What Happened

When running the `pytest` suite, the process hangs indefinitely, no cases is passed or failed. It is also unkillable. Opening `btop` reveals that the `cat` process is there hanging, and causing the `pytest` which calls `cat` as a subprocess hangs. The `cat` process is unkillable even with `SIGKILL`.

## How it is solved

At first, I just reboot the virtual machine using `systemctl reboot`, but that didn't work since it cannot be shut down. As a result, I forcibly shut down the VM, brought it up, and then use the command line directly to interact with the character device. The `cat` command also freezes so I had to forcibly shut it down again. I then look into the `kcalc_device_read` function in the source code.

```c
static ssize_t
kcalc_device_read(struct file *filp, /* see include/linux/fs.h   */
		  char __user *user_buf, /* buffer to fill with data */
		  size_t len, /* length of the buffer     */
		  loff_t *offset)
{
	ssize_t ret = 0;
	mutex_lock(&kcalc_buf_lock);
	if (*offset >= result_len) {
		ret = 0;
		goto out;
	}
	if (len > result_len - *offset)
		len = result_len - *offset;
	if (copy_to_user(user_buf, result_buf + *offset, len)) {
		ret = -EFAULT;
		goto out;
	}
	*offset += len;
	ret = len;
out:
	mutex_lock(&kcalc_buf_lock);
	return ret;
}
```

The problem is this function will never `unlock` the `mutex`, and all paths goes to the `out` label. The thread calls `mutex_lock` first, then do the reading or error to `out` label, but either way it will hit the second `mutex_lock(&kcalc_buf_lock);` statement.
The **same** thread sees the `mutex` is locked, so it just waits for it to unlock, which is held by itself.
This made it get put into `TASK_UNINTERRUPTIBLE`, so even `SIGKILL` takes no effect.
This is a `self-deadlock`, because the thread relocks the `mutex` held by itself.

## The actual fix in code

The fix is simple, just change the second `mutext_lock(&kcalc_buf_lock);` statement to `mutex_unlock(&kcalc_buf_lock);`. The `pytest` immediately worked, it won't hang and can finally confirm all tests are passing.

```diff
 static ssize_t kcalc_device_read(struct file *filp, char __user *user_buf,
 				  size_t len, loff_t *offset)
 {
 	ssize_t ret = 0;
 	mutex_lock(&kcalc_buf_lock);
 	if (*offset >= result_len) {
 		ret = 0;
 		goto out;
 	}
 	if (len > result_len - *offset)
 		len = result_len - *offset;
 	if (copy_to_user(user_buf, result_buf + *offset, len)) {
 		ret = -EFAULT;
 		goto out;
 	}
 	*offset += len;
 	ret = len;
 out:
-	mutex_lock(&kcalc_buf_lock);
+	mutex_unlock(&kcalc_buf_lock);
 	return ret;
 }
```

## What I learned

`mutex_lock` statements should be paired with a `mutex_unlock` statement, or it will go into a `self-deadlock`, just like how `malloc/kmalloc` and `free/kfree` should be paired.

This is also why the `goto` error handling pattern is used in the kernel.
If I just did early return whenever something goes wrong, then I would have to find which branch forgets to unlock itself.
But by using `goto` error handling pattern, only a single line change fixes all paths.

## Additional information

Additionally, I learned that for kernel version after `6.4`, which my debian VM uses `6.12`, there is a safer pattern, which uses `guard(mutex)(&kcalc_buf_lock)`, to prevent writing `mutex_lock` and `mutex_unlock` manually and triggering these kind of `self-deadlock`. We can also remove the goto statement completely, since even though it saves us debugging time, it is still not that intuitive to deal with, if coming from a higher level language like `python`.
