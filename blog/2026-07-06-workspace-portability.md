---
title: "Why Copying AI Workspaces Keeps Failing on Windows"
date: 2026-07-06
status: outline
---

# Why Copying AI Workspaces Keeps Failing on Windows

## Outline

### Hook (开头钩子)
> I switched laptops and lost everything. 139 conversations, all my configs,
> every skill I'd built. Gone. Weeks of AI collaboration vanished because I assumed
> copying a folder was enough.

### The Problem (问题)
- AI coding tools (Reasonix, Claude Code, Codex) store workspace data in hardcoded locations
- C: drive assumptions break on machine change
- Manual reconfiguration takes 30+ minutes per machine
- Most developers don't realize this until it's too late

### Failed Attempts (失败尝试)
1. **xcopy / robocopy** — Files copied, but hardcoded paths inside configs still pointed to old drive letter
2. **Windows Symlinks (mklink /D)** — Cross-device symlinks require admin privileges and don't work on exFAT USB drives
3. **Manual export/import** — No export feature exists; rebuilding from scratch is the only option
4. **Cloud sync (OneDrive/Dropbox)** — Syncs the files but doesn't rewrite paths; broken references follow you

### The Observation (洞察)
- The problem isn't copying files — it's path semantics
- Reasonix 0.5X and 1.X use different storage architectures:
  - 0.5X: HOME / USERPROFILE redirection + path-hashed memory directories
  - 1.X: APPDATA + NTFS Junction
- NTFS Junction is the only Windows-native mechanism that works cross-device without admin rights
- Path remapping must happen at launch time, not at copy time

### The Solution (方案): Portakit
- Detects current path on any drive/folder
- Creates NTFS Junction from APPDATA to portable location (1.X)
- Or redirects HOME / USERPROFILE and patches configs (0.5X)
- One bat file, double-click, done

### Results (结果)
- 17 Stars, 60+ clones in 3 days
- Used by Reasonix community for cross-device migration
- Zero config — plug USB, launch, code

### Key Takeaway (核心洞察)
> Portability isn't about copying files. It's about abstracting paths.

### Call to Action
→ [Portakit](https://github.com/CS-Faith/reasonix-portakit) — Make your AI workspace portable today.

---

## Writing Notes

### Target audience
Developers who use AI coding tools daily and have never thought about workspace portability — until they lose everything.

### Tone
First-person, battle-tested, no fluff. Show the scars.

### Visuals needed
- Before/After: original error vs Portakit-launched workspace
- Architecture diagram: how NTFS Junction redirects APPDATA
- Terminal recording: 3-step demo (copy → double-click → working)

### Distribution plan
- Week 5: Finalize draft
- Week 6: Publish on cs-faith.github.io + Reddit r/ClaudeAI + r/cursor
