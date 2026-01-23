---
layout: post
display: Porting Kilo to Rust
title: rust-kilo-port
categories: projects
tags: [Rust, Porting, C, Terminal, TUI]
github: https://github.com/smithjjoseph/rust-kilo-port
permalink: /:categories/:title
---

It is widely publicised that Rust as a programming language has the highest satisfaction amongst programmers that have used it. For this reason, I decided to delve into using Rust.

I decided that it might be fun to port kilo to Rust. Kilo is a small text editor written in C. It is intended to mimic the behviour of terminal-based text editors like nano.

<!--end_excerpt-->

My intention when porting this code is to remain faithful to the original source code, not use any external crates (Rust's external packages), and try to make the code as Rust-y as possible. My main aim is to learn how things are done in Rust and have fun!

## Notable Differences

### Low Level Calls
Kilo utilises a lot of syscalls to get information from the system: for example getting information about the terminal's state or size. In Rust there aren't necessarily built-in calls to get this information so in some cases it is necessary to make calls to C using libc. These OS calls are unsafe and as such require an `unsafe` block which isn't an issue as long as it is handled carefully.

### Global Variables
One of the major differences between C and Rust is that Rust discourages the use of global variables. Global variables are an important part of kilo. For example kilo utilises a global struct to hold the state of the terminal and then this is then referenced throughout the program.

To remedy this difference there are a couple of solutions including using `unsafe` blocks or using a mutex to make the global struct thread safe (`once_cell`). I instead opted to create a struct within main and pass it around to any function that might need it. This is much safer however it is quite verbose.