---
layout: post
title: "Customizing the Mac to behave like Xmonad"
date: 2012-06-03 05:48
comments: true
categories:
---

About 3 weeks ago, my Thinkpad died. It died of a fan error. Luckily, it caught me on Sunday when I didn't have to work. I had to get a working computer ready by the next day so that work does not get disrupted. Faced with less than a day of time, the logical choice is to get a new computer.

For computers, I only have eyes for Thinkpads and Macs. I wanted a Thinkpad. But the [prices of Thinkpads in Singapore](http://www.seeseelooklook.com/index.php) is ridiculously overpriced. It is about twice the [price in the US](http://www.lenovo.com/products/us/laptop/thinkpad/). As much as I wanted a Thinkpad, I decided on a Mac.

No doubt the Mac desktop experience is good. But it can never compare to [my Arch Linux setup](http://blog.liangzan.net/blog/2012/01/19/my-solarized-themed-arch-linux-setup/) where _everything_ is customized to _my liking_. I spent much effort getting my [Arch Linux](http://www.archlinux.org/) to not only look good and to work effectively. Faced with the conclusion that the next few years of my computing life is going to be on a Mac, I decided to make life easier for myself.

<!-- more -->

## Mac window management is a pain

My biggest pain point on the Mac is windows management. Every window has to be resized with a mouse. Every window has to be moved with a mouse. And every window is never full screen. Every time I used my mouse to manually move a window, I thought to myself, "I could do that in Mod-Shift-E in Xmonad". Another 3 seconds wasted moving windows. A good analogy is like mobile phones. Once you have experienced the convenience of mobile phones, you can never imagine going back to the no-mobile phone era. That is how good [Xmonad](http://xmonad.org/) is.

To mitigate my pain, I used a few techniques.

## Sizeup to the rescue

[Sizeup](http://www.irradiatedsoftware.com/sizeup/) app from Irradiated software solved some of those problems. It behaves like a psuedo [tiling window manager](http://en.wikipedia.org/wiki/Tiling_window_manager). It could make the app go full screen. It could make the app switch between monitors. All these are done with customizable key bindings. Besides that, it could tile multiple apps. I'm sold.

## No more cmd-tab stuttering with Alfred

My next pain point is the [Cmd-tab](http://en.wikipedia.org/wiki/Alt-Tab)(or on the PC: Alt-tab) stutter. I'm forced to do the Cmd-tab-tab-tab stutter _every time_ I want to switch to an app. In Xmonad, I usually put my apps in their designated [workspace](http://xmonad.org/tour.html#workspace). If I want emacs, I type Mod-2. If I want the shell, I type Mod-1. On the Mac, If I want emacs, I type Cmd-tab-tab-tab-tab. Oops I missed it, and it is Cmd-tab-tab-tab-tab again. Painful.

[Alfred](http://www.alfredapp.com/) app solved that problem for me. The [powerpack](http://www.alfredapp.com/powerpack/) allows you to add custom keybindings to run custom scripts. Which I did. I bound Option-1 to an Apple script that activates iTerm. Below is a sample of the script.

``` applescript
tell application "Emacs" to activate
```

EDIT: I realized Alfred can directly add a keybinding to the app. There is no need to write a custom apple script. Thanks to Jim Myhrberg for pointing it out.

Alfred app also acts as a better replacement for [dmenu](http://tools.suckless.org/dmenu/). It looks better, and it has fuzzy matching. The default dmenu don't have that.

## Emacs frame switching with frame-tag.el

I do most of my work on Emacs with multiple monitors. I usually have multiple frames open. In Emacs terminology a [frame](http://www.gnu.org/software/emacs/manual/html_node/emacs/Frames.html) is equivalent to the window. When I activate Emacs on the Mac, all my frames get focus. On Xmonad, I just push the frame to a different workspace and the problem of frame switching goes away. On the Mac, I have to do another C-x 5-o stutter to get to the frame I want. Painful.

Rather than whining about it, I solved my own problem. Being on Emacs means you can customize things. Hence I wrote [frame-tag.el](https://github.com/liangzan/frame-tag.el). It added M-1, M-2, etc keybindings to Emacs. That allowed me to switch to frames easily.

## My last pain point

Sometimes I want to show one emacs frame together with the shell. I cannot do that with a single keybinding on the Mac. On Xmonad, I can easily do that since each frame occupies a different workspace. It is a mere selection of the workspace on different screens. On the Mac I still had to use the mouse to focus on the application. I tried some clumsy workarounds. That included [simulating mouse clicks](http://www.bluem.net/en/mac/cliclick/) with key bindings. It didn't work well.

## In conclusion

A good developer may become productive on any platform. But these little performance enhancements adds up to make you faster. It matters. I feel that a customized Linux operating system is still the best choice for a friction free development experience.

A few days ago, I had an epiphany. All these cross platform pain would go away naturally if I had shifted more of my workflow to [The Emacs OS](http://c2.com/cgi/wiki?EmacsAsOperatingSystem). Window management problems gone. Cmd-tab stuttering gone. And you can do everything with key bindings. But still I believe I'll be back on Xmonad some day.
