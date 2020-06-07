---
layout: post
title: "Emacs vs Vim"
date: 2011-01-20 16:46
comments: true
categories:
---

I'm one of those who have tried both Vim and Emacs. I've always wanted to write a post to share my experiences on these 2 venerable text editors. Right now, my primry editor is Emacs. Not to say Vim is not good, but for Emacs fits my setup better. I'm typing this article on Emacs if you're curious.
First a little history. I started with Vim first. Once I got through the steep learning curve, it felt fast. I was typing faster. Vim made me saw how inefficient I was earlier. From time to time, I will see articles or comments that mention Emacs. My curiosity grew. I started trying out Emacs. When I started learning it, I forced myself to use only emacs for a week. After that week, I was able to memorise most of the basic key strokes. So after about 2 years of using Emacs, I think its the right time to summarise my opinions.

<!-- more -->

Comparing the both of them is not fair. They have a different design philosophy. When we use Vim, the mindset is 'I want to type in the shortest time, and in as few keystrokes possible'. When we use Emacs, the mindset is 'I want to use Emacs for everything'.

## Key Bindings
Vim keybindings are economical. But it can be cryptic. It is designed for the fingers. Emacs is designed for the brain. Key bindings can be executed through the minor modes which maps directly to an english phrase. Like 'comment-region', 'replace-string'. For Vim that would be "+gp'. As you can see, you are unable to guess just by reading what a Vim command can do.

## Placement of key bindings
A good example of the difference in philosphy is the movement buttons. Vim places them in a line, right next to each other(GHJK). Emacs use C-n, C-p, C-f, C-b. It maps to next, previous, forward and back. Vim designs for the fingers. Emacs design for the brain. For emacs you won't have to memorise as much, but your fingers suffer from stretching.

Another good thing about emacs is there're other software that correlates their default key bindings with emacs. screen, tmux, zsh, etc has very similar keybindings to emacs. It shortend the learning curve considerably.

## Window management
The killer feature that made Emacs my primary editor is the window management. Emacs allows me to have multiple frames. All the frames have access to the same buffers. This allows me to switch my buffers freely between the frames. That is very helpful if you use multi monitors. I cannot replicate the same behaviour with Vim. I'm stuck with the same buffers per Vim instance.

Otherwise, both editors are about equal when it comes to customizability and functions. For me, the above are the more obvious differences
