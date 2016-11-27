---
layout: post
title:  "Linux Development on Windows"
date:   2016-11-28 12:00:00 -0600
categories: development windows
author: Charles
description: >
  Use Windows Subsystem for Linux for development.
---

## Introduction: WSL is real Linux. Sorta.

Windows Subsystem for Linux came with the Win 10 Anniversary Update in July 2016, though
that version was hardly usable. It was limited to Ubuntu 14.04 and was missing most
of the system calls that would have made it useful for any sort of web development.

But that's not quite the same story on the fast ring. The Microsoft team has been
working through all of the various system calls that people are trying to use as they've
found them. Every week or two, a big batch of fixes comes out and we get to try our
development usage running again. We've even got 16.04 now.

See, it's real Linux, running alongside Windows, but it's mostly supported via various
system calls that give Linux access to the hardware that it's running on. This means
that we'll be fighting, for a while, against what's missing. It's a few steps forward,
find what breaks, debug, see if there's a workaround, then wait a few weeks for it
to be fixed if not.

But the good news is that we're at the stage where most simple use cases are working.
Stock Rails, Python, Java, and maybe Node apps work now, which gives hope that we're
getting close.

## Setup suggestions

To develop with Bash on Windows, you should join the insider program and join up
on the fast ring. You might be able to get by with an older build, but it's only
recently that I feel enough is supported to really get by.

If you need a login shell (I do), this can be added via changing the shortcut to Bash
to be a login shell.

## Providing services

My recommendation is to provide your database, redis, or any other background services
via Docker. The performance is good, especially when you use a Hyper V capable processor,
and it beats trying to debug running those services directly in the terminal.

Install Docker for Windows. Install Kibana. Provision services via the UI. You can
also install docker manager in Bash and connect to the the Windows service remotely.

If you're a vim/terminal developer, then you can stop here. There might be a few
issues that you run into with various tools, but for the most part, it's good to
go. Go have fun.

## Development environment

Personally, I like to use an external editor. Currently writing this blog in Atom,
which I have been happy with. You can use anything you like, though, as long as you
can access the files. That's where setting up WSL gets to be a bit tricky.

There are two different file systems used by Linux. VolFS is a proper Linux file system.
You can find it under Windows, but you really, really don't want to as a Windows
app doesn't know how to write all the flags that Linux expects. Don't even try. This
is why vim devs have a bit of an advantage. VolFS performance is pretty good and
supports all the necessary file operations to be useful.

The other side of that coin is DriveFS. This is exposed in Bash under /mnt/c (and d
and e and f...). DriveFS will eventually be a fully supported compatibility layer
but for now it has a few missing calls and is slower. Still, it's what we have to use.

My recommendation is to create a project directory under /mnt/c somewhere (I've got
mine in my Windows user's documents directory) and symlink it into /home/user.

And that's it. Clone your project, start your server, and start coding. With luck
it'll all work and you no longer need to dual boot (or not ever boot into Windows).
If you don't have luck, keep reading.

## Limitations & Debugging

As I mentioned many times earlier, there are a few big blockers to development. Fortunately,
Microsoft has been really responsive to confirming and fixing any issues that have
been found.

The first blocker you might run into could simply be the speed of file access in
DriveFS. Not really much can be done on that other that wait for it to get faster.
Sorry.

After that, you'll probably run into a bug at some point. Something that should work
just won't work. Head to WSL issues. Run strace to find what failed.
