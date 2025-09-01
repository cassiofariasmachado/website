---
title: "How I organize and version my configs"
author: "Cássio"
authorAvatarPath: "/profile.jpeg"
date: "2025-08-31"
summary: "Overview of how I structure and version my development environment configs — including dotfiles, shells, terminal, and editor configs."
description: "Overview of how I structure and version my development environment configs — including dotfiles, shells, terminal, and editor configs."
toc: false
readTime: true
autonumber: false
math: false
tags: []
showTags: true
hideBackToTop: false
hidePagination: false
fediverse: "@cassiofariasmachado@mastodon.social"
---

Hello everyone! 👋

In this post, I’d like to share how I organize and version my configuration files.

With this setup, I keep the following configs tracked in Git:

- **Dotfiles**: configuration files that start with a `.` and usually live in your home folder;  
- **Shell configurations**: PowerShell and ZSH;
- **Editor configurations**: currently just Neovim.

## How it works

For managing my `dotfiles`, I use [dotbot](https://github.com/anishathalye/dotbot) tool. It greatly simplifies the process and makes it easy to keep everything under version control.

My shell configurations live in separate repositories, one for PowerShell and another for ZSH. They are still referenced inside my `dotfiles` repo, but I keep them split out mainly for organization.

I also use [Oh My Posh](https://ohmyposh.dev) as prompt theming engine for both shells, with theme configurations stored in a dedicated repo. This repo is also integrated with my `dotfiles`.

## Repositories

- [dotfiles](https://github.com/cassiofariasmachado/dotfiles): My Dotfiles;
- [posh-config](https://github.com/cassiofariasmachado/posh-config): PowerShell Core configs, aliases, etc;
- [zsh-config](https://github.com/cassiofariasmachado/zsh-config): ZSH configs, aliases, etc;
- [omp-themes](https://github.com/cassiofariasmachado/omp-themes): Oh My Posh themes;
- [wt-config](https://github.com/cassiofariasmachado/wt-config): Windows Terminal configs (not yet integrated into `dotfiles`).

## Next steps

- Add other `dotfiles` to version control:
    - Visual Studio Code;
    - Integrate Windows Terminal configs currently versioned [here](https://github.com/cassiofariasmachado/wt-config).