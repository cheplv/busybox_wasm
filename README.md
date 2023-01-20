
<details>
    
  <summary>Original README</summary>

> ## BusyBox + Emscripten + nanozip + diff3
> 
> Inspired by https://github.com/tbfleming/em-busybox and https://github.com/tbfleming/em-shell, this repo contains build script of BusyBox for WebAssembly without being a full fork of BusyBox, so upgrading to a new version of BusyBox is easier.
> 
> In addition to BusyBox build script, this repo also contains two [custom](https://git.busybox.net/busybox/plain/docs/new-applet-HOWTO.txt) BusyBox applets:
> - [nanozip](https://github.com/vadimkantorov/nanozip) - [miniz](https://github.com/richgel999/miniz)-based imitation of `zip` utility: `busybox nanozip [-r] [[-x EXCLUDED_PATH] ...] OUTPUT_NAME.zip INPUT_PATH [...]`.
> - [diff3](https://github.com/openbsd/src/blob/master/usr.bin/diff3/diff3prog.c) - OpenBSD-based implementation of diff3: `busybox diff3 [-exEX3] /tmp/d3a.?????????? /tmp/d3b.?????????? file1 file2 file3`
> 
> [`em-shell.c`](https://github.com/tbfleming/em-shell/blob/master/runtime/em-shell.c), [`em-shell.h`](https://github.com/tbfleming/em-shell/blob/master/runtime/em-shell.h), [`em-shell.js`](https://github.com/tbfleming/em-shell/blob/master/runtime/em-shell.js), [`arch/em/Makefile`](https://github.com/tbfleming/em-busybox/blob/master/arch/em/Makefile) are taken from excellent [tbfleming/em-shell](https://github.com/tbfleming/em-shell) and [tbfleming/em-busybox](https://github.com/tbfleming/em-shell) by [Todd Fleming](https://tbfleming.github.io/).
> 
> Patches not used for now:
> - https://github.com/tbfleming/em-busybox/commit/8c592ed5e13a7c35e0e318112bbdbc281798b6d7
> - https://github.com/tbfleming/em-busybox/commit/5fbe7c016af61b21c073652fed3b4ee4d744238d
> 
> 
> ```shell
> # native version 
> make build/native/busybox
> 
> # wasm version
> make build/wasm/busybox_unstripped.js
> ```
> 

</details>

[![Nightly build](https://github.com/k8188219/busybox_wasm/actions/workflows/make.yml/badge.svg?branch=master)](https://github.com/k8188219/busybox_wasm/actions/workflows/make.yml)

## BusyBox WASM

BusyBox compiled to WASM with emscripten.

* Runs in Browser & Web Workers.
* Currently no `sh` or `ash` support.
* Currently defined functions

  ```
  BusyBox v1.32.0 (2023-01-20 07:19:55 UTC) multi-call binary.
  BusyBox is copyrighted by many authors between 1998-2015.
  Licensed under GPLv2. See source distribution for detailed
  copyright notices.

  Usage: busybox [function [arguments]...]
     or: busybox --list
     or: function [arguments]...

    BusyBox is a multi-call binary that combines many common Unix
    utilities into a single executable.  Most people will create a
    link to busybox for each function they wish to use and BusyBox
    will act like whatever it was invoked as.

  Currently defined functions:
    cat, clear, cp, diff3, echo, false, ls, mkdir, mv, nanozip, pwd, rm,
    sleep, tar, touch, true
  ```
  
  
## Installation

Download an artifact from [last successful build](https://github.com/k8188219/busybox_wasm/actions).

Append following lines to `busybox_unstripped.js`

```js
// busybox_unstripped.js
// ...

var resolve

Module.callMain = callMain
Module.onRuntimeInitialized = () => resolve()
thisProgram = 'busybox'

await new Promise(r => resolve = r)

export default Module
```

## Usage

```js
import busybox from "./busybox_unstripped.js"

busybox.callMain(["ls"])
busybox.callMain(["ls", "-al"])
```


