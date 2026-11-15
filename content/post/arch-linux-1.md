---
date: "2026-10-24T04:10:29+08:00"
draft: false
title: "Arch Linux (0)"
cover:
  image: "https://blog-assets.hoshiyomi-dev.work/archlinux-simple-cover.png"
  alt: "archlinux simple"
  caption: "archlinux"
categories: ["Linux Stuff"]
tags: ["linux", "hyprland", "gslapper", "linux-ricing", "unixporn"]
series: ["Arch Linux"]
---

## Arch Linux

[Arch Linux](https://archlinux.org/) is a Linux distribution that is known for minimalistic, DIY philosiphy, for arua farming
and finally being able to say "I use arch btw".

Some users are attracted to it because of the tiling window manager show cases in [r/unixporn](https://www.reddit.com/r/unixporn/top/?t=all) on reddit.

Personally, I use [Arch Linux](https://archlinux.org) (btw) and [Hyprland](https://hyprland.org).

## What are those and how do I install?

Nowadays, people usually just use `archinstall` script to get things done.

Back when I installed it, there were no such script. I followed the [installation guide](https://wiki.archlinux.org/title/Installation_guide).

What I learned from it is simple:

> READ THE DOCS BEFORE YOU ASK QUESTIONS

That's it.

Well actually I also learned some knowledge about how a computer works, including

- File systems, which are required for, well, your files.
  - These are done through first formatting a drive, partitioning, and then formatting the partitions into desired file systems
  - I use `ext4` for example, some others are `btrfs`, `ntfs` (Windows NT), and no there are no `dfs` nor `bfs` as file system (as far as I know).
- Bootloaders, which are required for, well, booting your PC and loading your kernel.
- Network configurations, which goes through some basics of networking.
- CLI usage.
- `systemd` services.

## Why Arch linux?

The question should be why not Arch Linux. Arch Linux may not be beginner friendly, in fact, even for CS majors, they will still struggle when installing, configuring. Every one starts the same. There's no reason to avoid to learn something new, well, also no reason to not avoid though.

Pros:

- Simple
- Full control of your system
- Latest packages (mostly)
- Rolling release

Cons:

- Hard to setup
- Updates too frequently, may break stuff
- Hard to pin down a version of python for running local AI, need to use `uv` or `pyenv`

## Why Hyprland

Pros:

- Keyboard driven development workflow
- Integrates well with terminal development workflow
- Highly customizable
- Clear documentations
- Large community
- Wayland!

Cons:

- Takes a long time to setup
- May get into configuration hell
- Some feature on desktop environments might not just work out of the box

## Conclusion

No conclusion. I said so. This is my blog.

Also check out my Linux dotfiles <https://github.com/xDDoubleTea/dotfiles>

## Misc

Wallpaper via <https://github.com/Nomadcxx/gSlapper>

- Static
  ![wallpaper](https://blog-assets.hoshiyomi-dev.work/blog-archlinux-1-1.png)

- Animation
{{< video src="https://blog-assets.hoshiyomi-dev.work/blog-archlinux-1-kei.mp4" >}}

It plays an `mp4` file and loops. I grabbed it through wallpaper engine, but any mp4 file would work, so you don't have to buy wallpaper engine.

> **Note:** gSlapper on NVIDIA grew about 56 MB/h with a video wallpaper. The leak was in
> NVIDIA's egl-wayland2 1.0.2, not gSlapper: under explicit sync it never dispatched its
> per-surface queue, so every frame's `wl_buffer.release` event piled up. Fixed upstream in
> [egl-wayland2 #56](https://github.com/NVIDIA/egl-wayland2/pull/56) (merged 2026-10-20,
> due in 1.0.3). Until then, force egl-wayland v1 for gSlapper:
>
> ```sh
> __EGL_EXTERNAL_PLATFORM_CONFIG_FILENAMES=/usr/share/egl/egl_external_platform.d/10_nvidia_wayland.json gslapper ...
> ```
