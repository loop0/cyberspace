---
title: "Running veilid server and cli on macos"
date: 2023-09-01T14:21:19-04:00
draft: false
---

Hello, I decided to start learning how [veilid](https://veilid.com/) works. So the first thought was to get `veilid-server` running on my main machine, which is a macbook pro m1 running macos. I'm writing this as a guide to myself in the future in case I need to do this again, but feel free to follow it in case it helps.

So, first thing is to clone the veilid repository with the following commands:

```sh
$ git clone --recurse-submodules https://gitlab.com/veilid/veilid.git
$ cd veilid
```

After that, I run `cargo build -p veilid-server` but that didn't work right away, turns out my rust version was a little out-of-date. So, the command to get it updated is:

```sh
$ rustup update
```

If you don't have rust installed yet, you can do it following rust's guide [here](https://www.rust-lang.org/tools/install).

After updating rust, my build was still failing, turns out I had to install a couple more dependencies using brew:

```sh
$ brew install protobuf capnp
```

And now it is time to give `veilid-server` build a try:

```sh
$ cargo build -p veilid-server
```

Once the build finishes your binary will be at `target/debug/veilid-server`, happy hacking!

Ps.: To build `veilid-cli` you just have to do `cargo build -p veilid-cli`