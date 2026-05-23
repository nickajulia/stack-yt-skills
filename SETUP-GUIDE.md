# STACK: 4 YouTube Skills for Claude — Setup Guide

Four skills I built while building Fuse, for getting a fast read on any YouTube channel.
This guide gets you running in about 30 seconds. No signup, no email.

## What is in this download

```
stack-yt-skills/
  skills/
    yt-channel-diagnostic/SKILL.md   Baseline views, top outliers, consistency score
    yt-outlier-hunter/SKILL.md       Every video ranked by how hard it beat baseline
    yt-title-rewriter/SKILL.md       5 rewrites matched to what your audience rewards
    yt-pattern-reader/SKILL.md       What connects your winners, what kills your losers
  full-teardown-prompt.md            Runs all 4 in one pass
  SETUP-GUIDE.md                     This file
```

Each skill works on a channel's recent uploads, the window YouTube exposes publicly.
Paste a channel URL and it pulls the data itself. If that fails, it asks you to paste a
video list. Either way you get a read.

## The fastest way (works on any Claude, free or paid)

No install needed.

1. Open any skill file, for example `skills/yt-channel-diagnostic/SKILL.md`, in any text editor.
2. Select all, copy.
3. Paste it into a new Claude chat.
4. On the next line, paste a YouTube channel URL.
5. Send.

That is it. The skill file is just instructions Claude follows. Pasting it works everywhere.

## Install as a real skill (Claude.ai)

If you want the skills to run automatically without pasting them each time:

1. In Claude.ai, open Settings, then Capabilities, then Skills.
2. Add a skill and upload a skill folder (the folder containing its `SKILL.md`).
3. Repeat for each of the 4.

Once installed, just ask Claude things like "diagnose this channel: [URL]" and the right
skill runs on its own.

## Install in Claude Code

Copy the 4 skill folders into your skills directory:

```
cp -r skills/yt-* ~/.claude/skills/
```

They register automatically. Then ask Claude Code to diagnose, hunt outliers, rewrite a
title, or read the pattern on any channel.

## How to run each skill

Once a skill is loaded (pasted or installed), give it a channel:

- **Channel Diagnostic** — "Diagnose this channel: youtube.com/@channel"
- **Outlier Hunter** — "Find the outliers on youtube.com/@channel"
- **Title Rewriter** — "Rewrite this title: [your title]. Channel: youtube.com/@channel"
- **Pattern Reader** — "Read the pattern on youtube.com/@channel"

No URL handy? Paste your last ~15 videos as a list (title, view count) and the skill runs
on that instead.

## The full teardown

`full-teardown-prompt.md` runs all 4 skills in sequence on one channel and returns a single
combined report: diagnostic, then outlier hunt, then pattern read, then a title rewrite.
Open that file, copy the prompt, paste it into Claude, drop in a channel URL, send.

## A note on accuracy

These skills read the channel's recent uploads from YouTube's public feed, about the last 15,
with Shorts set aside so they do not distort the long-form baseline. View counts are exact.
Likes are approximate. Comments are not in the public feed. It is a sharp read on recent
form, not a full channel history.

That is the line between these prototypes and the real thing. Fuse runs this same kind of
analysis continuously, across your whole channel, with full history and all three signals:
getfused.io. Free tier exists.
