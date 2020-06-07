---
layout: post
title: "My Solarized themed Arch Linux Setup"
date: 2012-01-19 15:36
comments: true
categories:
---

![Arch linux solarized screenshot with emacs urxvt tmux xmonad](https://s3.amazonaws.com/static.liangzan.net/blog/arch-linux-solarized-screenshot-with-emacs-urxvt-xmonad.png)

The screenshots received much interest from my friends. I am going to show you how to build it. Long story short, use the same color theme for your applications.

<!-- more -->

## OS: Arch Linux

![Arch linux xmonad solarized screenshot with blank desktop](https://s3.amazonaws.com/static.liangzan.net/blog/arch-linux-solarized-screenshot-blank-desktop.png)

Obviously only Linux or BSD gives you this much freedom to customize your desktop. I'm using Arch Linux. Other distros should yield the same result. The advantage of using Arch Linux is it gives you a blank slate to work on. If you use all-in-one distros like [Ubuntu](http://www.ubuntu.com), you would have the extra work of stripping out the desktop environment.

## Color theme: Solarized
There're some very good color themes available. Here're some very good ones.

- [Solarized](http://ethanschoonover.com/solarized)
- [Zenburn](http://slinky.imukuppi.org/zenburnpage/)
- [IR_Black](http://blog.toddwerth.com/entries/13)

Choose one which you like. I'm using Solarized. Not all color themes are created equally. Popular themes have more tooling support. For Solarized, it has been ported to vim, mutt, shell, and emacs. It makes it easier to apply the same color theme to other applications.

## Font: Inconsolata
As with color themes, font is very much a personal preference. Choose a good monospaced font to use for all your apps. I'm using the [Inconsolata](http://levien.com/type/myfonts/inconsolata.html) font.

There are other good recommendations [here](http://hivelogic.com/articles/top-10-programming-fonts/)

## Window manager: XMonad
I'm using [XMonad](http://xmonad.org/). I wanted a [tiling windows manager](http://en.wikipedia.org/wiki/Tiling_window_manager) as I wanted to maximise my screen estate. I chose XMonad because of the way it handles windows on multi monitors. Each screen shows an independent workspace. I have control over which application should appear on which screen. For example, I can show a movie on my second monitor while switching workspaces only on the laptop screen. The movie does not get affected. OS X, Windows, Gnome, KDE treats both screens as a single workspace. Nothing wrong with that. But I had to manually move apps from screen to screen and do a lot of Alt-Tab window cycling. With XMonad, I no longer need to do that. I just switch workspaces. XMonad allows you to configure apps to automatically go to designated workspaces. You get the added advantage of knowing exactly where which app is.

This [repository](https://github.com/vicfryzel/xmonad-config) is very well documented. My config is almost a carbon copy of it.

## Status bar: Xmobar
[Xmobar](http://projects.haskell.org/xmobar/) works well with my Window Manager(Xmonad). Configuring it is very easy. You should be able to achieve the same result with [dzen2](https://sites.google.com/site/gotmor/dzen) too. Dzen has more features though. Xmobar is a plain text status bar. Here's my [.xmobarrc](https://gist.github.com/1643647). The gmail checker is a simple hand written [ruby script](https://gist.github.com/1643664). The colors used are picked from the solarized theme.

## Termainal emulator: urxvt

![Arch linux xmonad solarized screenshot with urxvt](https://s3.amazonaws.com/static.liangzan.net/blog/arch-linux-solarized-screenshot-with-urxvt.png)

I'm using [urxvt](http://software.schmorp.de/pkg/rxvt-unicode.html) as my terminal emulator. urxvt is highly customizable. I don't think the default terminal emulators that come with Gnome, KDE offer the same room for customizability as urxvt. It should not be a problem to install urxvt in your distro though. Just apply the color theme to your terminal. And it should merge nicely into the environment.

Here is my [config](https://gist.github.com/1643690) for urxvt

## Text editor: Emacs

![Arch linux xmonad solarized screenshot with emacs](https://s3.amazonaws.com/static.liangzan.net/blog/arch-linux-solarized-screenshot-with-emacs.png)

Be it Vim or Emacs, you can easily apply a color theme for it. There's the Solarized theme for Vim as well. Just follow the instructions and install the appropriate color theme for your text editor. For emacs, you need to use the color theme package. There's a separate [solarized color theme package](https://github.com/sellout/emacs-color-theme-solarized). Install it and it'll merge perfectly with the status bar.

## OS integration
To take it further, I made my wallpaper integrate into the status bar. I also used dark themes for Chrome and Firefox. Similarly for web apps like Gmail, I try to use a dark theme to merge in with the desktop. This makes the entire computing experience very pleasing to the eye.
