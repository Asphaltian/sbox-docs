---
title: "S&box Documentation"
icon: "🍌"
created: 2023-10-26
updated: 2026-09-25
---

# S&box Documentation

# About

S&box is coded in C#. Under the hood, it uses the Source 2 engine (CS2, HL:Alyx, DOTA2) and some of its systems: rendering, resources, physics, and audio.

When you create games and addons in s&box, you will be creating them in C#.

We have developed a hotload system which is capable of compiling & hotloading your changes to code within a few milliseconds, which negates the need for a scripting language.

# Scenes

We use a scene system, similar to Godot and Unity. This allows faster iteration, without everything being code-based. The scene system aims to make how everything works more transparent, by being easily visible, and easily accessible.

# Standalone Games

You can export your game as a standalone executable. Standalone distribution is currently in preview and requires Valve's approval and a license from Facepunch. Facepunch takes no engine royalties or revenue share; Steam's standard terms and revenue share still apply. See [Exporting Standalone](/exporting/index.md) for the approval process and release requirements.

## Reporting Issues

Issues and feature suggestions should be posted in [the sbox-public repo](https://github.com/Facepunch/sbox-public).

Please see [Reporting Errors](/getting-started/reporting-errors.md).

## Getting Started

Please see [First Steps](/getting-started/first-project.md).
