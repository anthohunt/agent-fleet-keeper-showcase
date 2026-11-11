**English** · [Français](README.fr.md)

**Anthony Hunt** · [Project page on ingevision.cloud](https://ingevision.cloud/en/ai-agents/?p=agent-gardien-du-parc) · [LinkedIn](https://www.linkedin.com/in/anthony-hunt-3948a9196) · [GitHub profile](https://github.com/anthohunt)

> Showcase repository: the code and data of this project are private; this README describes it.

# Agent - Workstation fleet keeper

A permanent agent that keeps a room of six shared GPU workstations running, and decides nothing on its own about who gets them: it hands them out on request, takes them back when work ends, and repairs what breaks.

## The problem

Six identical workstations (VR, Unity, local models) are shared between people and coding agents. Without a keeper, two agents start on the same machine, a forgotten job holds a workstation for days, a session locks itself and nobody can reach the machine, and the owner finds out when a demo fails.

## How it works

- **Ask, use, give back**: `parc demander -n 2 --besoin "what it is for"`, `parc rendre <workstation>`, `parc etat`. Each workstation has one holder and one stated purpose.
- **A door, not a rule**: without an allocation, SSH to a workstation is refused. Colleagues reach the room only through a small gateway service with three roles (administrator, operator, user).
- **No automatic priority**: when there are not enough machines, the agent lists who holds what and the owner decides. It never takes a machine away from running work without his order; the only exception is a machine whose holder has vanished and where nothing runs.
- **Rounds**: every 10 minutes a check for vanished holders and undeclared work; every 4 hours a health check (disk, GPU, network, Unity); a deep check every Sunday night.
- **The agent sleeps** until a signal arrives (breakdown, undeclared job, machine to hand back), handles it, then waits again. A breakdown it cannot fix becomes one line to the owner: the machine, the plain cause, what to do.

## What is built

In service since 4 November 2026 on 6 workstations: allocation and SSH door, colleague gateway, rounds and health checks, repair scripts (locked session, network link, guard against unwanted shutdowns), and a page showing the state of each workstation.

## What is public

This page only. The code and logs stay private.

## Tech stack

- Claude Code agent (permanent session)
- Python (allocation, rounds, health checks)
- Go (colleague gateway)
- PowerShell (scripts deployed on the Windows workstations)
- Tailscale, OpenSSH, systemd timers
