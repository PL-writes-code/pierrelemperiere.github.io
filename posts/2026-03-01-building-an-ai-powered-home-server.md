---
title: Building an AI-Powered Home Server
date: 2026-03-01
description: How I set up a dedicated SER8 mini PC running Arch Linux as a persistent AI assistant with Docker, automation hooks, and Telegram integration.
---

# Building an AI-Powered Home Server

The idea was simple: a small, silent box that runs 24/7, hosting an AI assistant that can research, write code, manage projects, and notify me when things are done.

## The Hardware

I went with an SER8 mini PC — small footprint, low power draw, enough grunt to run Docker containers and serve as a development box. It sits on my desk, barely audible, always on.

## The Stack

- **Arch Linux** with [Omarchy](https://github.com/omarchy) — a minimal, opinionated desktop setup
- **Docker** for isolation — the AI assistant runs in its own container with a mounted workspace
- **Claude Code** as the AI backbone — persistent sessions, tool access, full autonomy within the container
- **Telegram Bot** for notifications — the assistant pings me when tasks are complete
- **GitHub Actions** for deployment — blog posts get committed locally, pushed by host-side automation

## Why Not the Cloud?

Latency, control, and cost. A local box means instant file access, no egress fees, and full ownership of the environment. The AI assistant can read local docs, write to mounted volumes, and interact with the host system through well-defined interfaces.

## What It Does

The server runs continuously. When I drop a task file into the workspace, the assistant picks it up, processes it, and notifies me. It can:

- Write and publish blog posts from doc diffs
- Research topics and compile findings
- Build tools and scripts autonomously
- Manage its own memory across sessions

The constraint is the interface: everything flows through files, environment variables, and Telegram messages. No GUI, no voice, no real-time chat. Just async task processing.

## What's Next

More automation hooks, better task queuing, and expanding the assistant's toolkit. The goal is a system that handles the boring stuff so I can focus on the interesting stuff.
