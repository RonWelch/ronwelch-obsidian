---
title: Wasm-native Unix sandboxing for the web
source: https://wanix.dev/
author:
published:
created: 2026-07-24
description: Run and interact with real Wasm and x86 programs sandboxed in the browser. No server. Inspired by Plan 9.
tags:
  - clippings
  - plan9
  - Unix
  - WASM
---
[new Wanix 0.4 — wanix elements](https://github.com/tractordev/wanix/releases)

Run and interact with real Wasm and x86 programs entirely sandboxed in the browser. No server. Inspired by [Plan 9](https://en.wikipedia.org/wiki/Plan_9_from_Bell_Labs).

```
<wanix-term>
  <wanix-bind dst="rc.wasm" type="file"
    src="https://wanix.dev/extras/0.4.0-rc2/rc.wasm">
  </wanix-bind>
  <wanix-task cmd="rc.wasm" term start></wanix-task>
</wanix-term>
```
```
rc% ls
rc.wasm
rc%
```

## Basic starting points using Wanix Elements

Allocates a new `#ramfs` to the namespace root, binds in the rc shell and an inline file, then runs the shell task with a terminal. Try running:
- `ls`
- `cat shell.txt`

```
<wanix-term>
  <wanix-bind dst="." src="#ramfs/new"></wanix-bind>
  <wanix-bind dst="rc.wasm" type="file" src="https://cdn.jsdelivr.net/npm/wanix-extras@0.4.0-rc2/dist/rc.wasm"></wanix-bind>
  <wanix-bind dst="shell.txt" type="file">
    Hello, shell!
  </wanix-bind>
  <wanix-task cmd="rc.wasm" term start></wanix-task>
</wanix-term>
```

<iframe src="https://wanix.dev/recipes/run-shell.html"></iframe>

## Just a few tags bootstrap a Unix for the web

### Core — the building blocks

orthogonal primitives

`<wanix-task>` runs an executable in a namespace [docs](https://github.com/tractordev/wanix#wanix-task)

```
<wanix-task cmd="hello.wasm arg1 arg2" start>
  <wanix-bind dst="hello.wasm" type="file" 
    src="./hello.wasm"></wanix-bind>
</wanix-task>
```

Bind a Wasm binary into the namespace, then start it. `cmd` is the command line; `start` runs it headless as soon as the system is ready.

```
<wanix-namespace>
  <wanix-bind dst="." src="#ramfs/new"></wanix-bind>
  <wanix-bind dst="task.js" type="file" perm="0766">
    console.log("JS task running!");
  </wanix-bind>
  <wanix-task cmd="task.js" start></wanix-task>
</wanix-namespace>
```

Inline a JS file and run it as a task. Wanix supports task drivers for pluggable execution with built-in drivers for JS and Wasm.

`<wanix-term>` an xterm.js terminal for a task or vm [docs](https://github.com/tractordev/wanix#wanix-term)

```
<wanix-term>
  <wanix-bind dst="rc.wasm" type="file"
    src="/dist/extras/rc.wasm">
  </wanix-bind>
  <wanix-task cmd="rc.wasm" term start></wanix-task>
</wanix-term>
```

Typically tasks are started with a terminal allocation that `<wanix-term>` automatically wires up to. Its common to use `<wanix-term>` as the top-level element for a task.

```
<wanix-term for="namespace-id" path="#vm/1/term" raw></wanix-term>
```

Visual elements like `<wanix-term>` can be used outside a namespace for styling flexibility, they just need explicit `for` and `path` attributes to wire up to Wanix terminal.

`<wanix-vm>` runs a virtual machine in a namespace, powered by v86 [docs](https://github.com/tractordev/wanix#wanix-vm)

```
<wanix-vm mem="1G" start>
  <wanix-bind dst="#vm/v86" type="archive"
    src="/dist/extras/v86.tgz"></wanix-bind>
  <wanix-bind dst="." type="archive" 
    src="/dist/extras/wanix-linux.tgz"></wanix-bind>
</wanix-vm>
```

Boot a headless Linux VM by binding in the v86 emulator assets and the Linux system image. It will detect the Linux kernel and boot to a shell.

```
<wanix-term raw>
  <wanix-bind dst="#vm/v86" type="archive"
    src="/dist/extras/v86.tgz"></wanix-bind>
  <wanix-bind dst="." type="archive" 
    src="/dist/extras/wanix-linux.tgz"></wanix-bind>
  <wanix-vm export="ttyS0" term start></wanix-vm>
</wanix-term>
```

This is more typical where you would allocate and attach a terminal, though for VM consoles the terminal needs to be in `raw` mode.

If the image supports, `export="ttyS0"` can be used to export the internal namespace at `#vm/1/guest`.

`<wanix-namespace>` an explicit namespace container [docs](https://github.com/tractordev/wanix#wanix-system)

```
<wanix-namespace>
  <wanix-bind dst="." src="#ramfs/new"></wanix-bind>
  <wanix-bind dst="greeting.txt" type="file">
    Hello, world!
  </wanix-bind>
</wanix-namespace>
```

Use `<wanix-namespace>` to explicitly create a namespace. Using any other tag as the top-level element will implicitly create a namespace.

```
<wanix-namespace id="main" allow-origins="*">
  <wanix-bind dst="." src="#ramfs/new"></wanix-bind>
  <wanix-bind dst="exported.txt" type="file">
    exported data
  </wanix-bind>
</wanix-namespace>
```

Give the namespace an `id` and `allow-origins` so other pages can import this namespace using an import bind.

`<wanix-bind>` bind mount files, archives, and other namespaces into the namespace [docs](https://github.com/tractordev/wanix#wanix-bind)

```html
<wanix-namespace>
  <wanix-bind dst="." src="#ramfs/new"></wanix-bind>
  <wanix-bind dst="web" src="#web"></wanix-bind>
  <wanix-bind dst="hello.wasm" type="file" 
    src="./example/hello.wasm"></wanix-bind>
  <wanix-bind dst="note.txt" type="file">
    Hello from Wanix!
  </wanix-bind>
</wanix-namespace>
```

Bind can link names, allocate devices, or using `type="file"` it can fetch or inline files. Bind is a versatile primitive for building namespaces.

```html
<wanix-namespace>
  <wanix-bind dst="." type="archive" 
    src="./example/base.tgz">
  </wanix-bind>
  <wanix-bind dst="." type="archive" 
    src="./example/overlay.tgz">
  </wanix-bind>
</wanix-namespace>
```

Using `type="archive"` you can unpack a `.tar` / `.tgz` into a directory tree. Layer multiple archives or directory binds to the same `dst` to create a recursive union.

```html
<wanix-namespace>
  <wanix-bind dst="remote" type="import" 
    src="https://other.example/app.html#main">
  </wanix-bind>
</wanix-namespace>
```

Using `type="import"` you can import a remote namespace using 9P over WebSocket or as an embedded iframe.

### Extended — larger components

require additional assets

`<wanix-workbench>` embedded VS Code workbench as editor, file explorer, full IDE, or app shell [docs](https://github.com/tractordev/wanix#wanix-workbench)

```
<wanix-workbench assets="/workbench" term>
  <wanix-task role="shell" cmd="bin/sh"></wanix-task>
</wanix-workbench>
```

Embed a VS Code workbench backed by the Wanix namespace. The `assets` attribute is required. A task with `role="shell"` needs to be given to enable a terminal.

```
<wanix-workbench open="foo.txt" sidebar="never" 
    assets="/workbench">
  <wanix-bind dst="." src="#ramfs/new"></wanix-bind>
  <wanix-bind dst="foo.txt" type="file">
    Hello, foo!
  </wanix-bind>
</wanix-workbench>
```

Using attributes like `open` and `sidebar` you can control the initial state of the workbench. Here we simplify IDE to effectively be a single-file editor.

```
<wanix-workbench assets="/workbench"
    data-ext-myextensionA="/myextensionA"
    data-ext-myextensionB="/myextensionB">
    <!-- !! hypothetical, not supported !! -->
</wanix-workbench>
```

In the futre, you'll be able to add custom extensions and views to workbench and even live edit them within the workbench itself.

## Add Wanix to any page

You can grab the assets from the [latest release](https://github.com/tractordev/wanix/releases) or just use the CDN:

```
<script type="module"
  src="https://cdn.jsdelivr.net/npm/wanix@0.4.0-rc2/dist/wanix.min.js"></script>
```

the ideas behind wanix

19 min

## The spirit of Plan 9, in Wasm

Per-process namespaces, everything-is-a-file, and why a research OS from the 90s turns out to be the right model for the local-first web.

[▷ watch the talk](https://www.youtube.com/watch?v=kGBeT8lwbo0)