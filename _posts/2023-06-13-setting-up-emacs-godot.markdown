---
layout: post
title:  "Setting up Emacs for Godot Engine development"
categories: emacs godot
---

This is a quick blog post, in case any developer wonders how to configure their favourite OS to use with Godot development (the Engine code itself, not individual projects).

I previously had a tough time configuring Emacs for C/C++ projects, probably because I had no idea what I was doing, but I like to think that the problem was the LSP support on Emacs. Nowadays it's pretty much straightforward. The main requisite is following this guide:

[https://emacs-lsp.github.io/lsp-mode/tutorials/CPP-guide/](https://emacs-lsp.github.io/lsp-mode/tutorials/CPP-guide/)

To install the corresponding LSP packages on your Emacs. The main packages are `lsp-mode` (main LSP mode), `lsp-ui` (LSP UI support), `dap-mode` (for debugging), `company` (autocomplete) and `flycheck` (syntax checking). Remember to set up `clangd` first.

Ignore their `Project Setup` section for the Godot project. Instead, what you want to do is run `scons compiledb=yes compile_commands.json` on the Godot project root folder, so it generates the stuff LSP needs.

After setting up `clangd`, LSP and generating `compile_commands.json`, Emacs should behave just like any other IDE a normal sensible person would use if they don't like the THRILL of breaking their IDE configuration every now and then.

To be able to use the debug commands (`M-x dap-debug`) the tutorial points you to, you have to create a `launch.json` file in the Godot project root, the Debug launch command configuration. Mine looks like this:

```
{
		"name": "Launch Project",
		"type": "cppdbg",
		"request": "launch",
		// Change to godot.linuxbsd.tools.64.llvm for llvm-based builds.
		"program": "${workspaceFolder}/bin/godot.linuxbsd.editor.dev.x86_64",
		// Change the arguments below for the project you want to test with.
		// To run the project instead of editing it, remove the "--editor" argument.
		"args": [ "--editor, "--path", "path-to-your-godot-project-folder" ],
		"stopAtEntry": false,
		"cwd": "${workspaceFolder}",
		"environment": [],
		"externalConsole": false,
}
```

That's the VSCode `.json` from the Godot documentation guide, but replacing `lldb` with `cppdbg`, and removing the `preLaunchTask` property (you'll have to create a "build" task `.json` if you want a `preLaunchTask`). After that, you should be able to add breakpoints with `M-x dap-breakpoint-add` and run `M-x dap-debug` successfully.

### Why Emacs, though? Isn't VSCode much easier to configure and use for C++ development?

Yes.
