# STACK: 4 Claude Skills for YouTube Channel Analysis

Four installable Claude skills that read a YouTube channel's recent uploads and produce the kind of analysis YouTube Studio doesn't.

Built by Nick Julia while building [Fuse](https://getfused.io), a creator analytics platform. These are the prototypes. Fuse is the production version. Both have a job.

## What's in the bundle

| Skill | What it does |
|---|---|
| `yt-channel-diagnostic` | Paste a channel URL. Get median baseline views, top 5 videos with multiplier vs baseline, a 0-100 view consistency score, and a 2-3 sentence plain read. |
| `yt-outlier-hunter` | Every recent video ranked by signal strength (z-score), not raw views. Dual-signal (views + rating proxy) by default; triple-signal if you paste comment counts. |
| `yt-title-rewriter` | Paste a weak title and a channel URL. Learns the title pattern from the channel's own top performers, then gives 5 rewrites matched to that pattern. |
| `yt-pattern-reader` | Looks at the best and worst of a channel's recent uploads and writes the read: what connects the winners, what kills the losers, one actionable takeaway. |

Plus `full-teardown-prompt.md`, a chained prompt that runs all four in one pass on a single channel URL.

## How it gets the data

The skills fetch each channel's last 15 uploads via the YouTube RSS feed (`youtube.com/feeds/videos.xml?channel_id=UC...`). View counts are exact. Shorts and same-day uploads are set aside so they don't distort the baseline. If the feed fails, the skills fall back to asking you to paste a video list.

The math mirrors the calculators used inside Fuse: median baseline, z-score outlier detection (`|z| >= 1.5`), inverse coefficient-of-variation for the consistency score.

## Install

Three ways, fastest first.

### 1. Paste (works on any Claude, no install)

Open any skill file (e.g. `skills/yt-channel-diagnostic/SKILL.md`), copy the entire file, paste it into a new Claude chat, then add a channel URL.

### 2. Claude.ai skills

Settings → Capabilities → Skills → Upload skill. Upload each of the four skill folders.

### 3. Claude Code

```bash
git clone https://github.com/nickajulia/stack-yt-skills.git
cp -r stack-yt-skills/skills/yt-* ~/.claude/skills/
```

Then ask Claude things like "diagnose this channel: youtube.com/@channel".

See `SETUP-GUIDE.md` for the full walkthrough.

## How to run

Once a skill is loaded, drop a channel URL:

- **Diagnostic**: "Diagnose this channel: youtube.com/@channel"
- **Outlier Hunter**: "Find the outliers on youtube.com/@channel"
- **Title Rewriter**: "Rewrite this title: [your title]. Channel: youtube.com/@channel"
- **Pattern Reader**: "Read the pattern on youtube.com/@channel"

No URL handy? Paste your last ~15 videos as a list (title, view count). The skills work on either input.

## The full teardown

`full-teardown-prompt.md` runs all four skills on one channel in one pass and returns a combined report: diagnostic, outlier hunt, pattern read, title rewrite. Open the file, copy the prompt, paste it into Claude, drop in a channel URL.

## A note on accuracy

These read the last ~15 uploads YouTube exposes publicly. View counts are exact. Likes are approximate (the RSS feed gives a rating count, not a clean like count). Comments are not in the public feed.

It is a sharp read on recent form, not a full channel history. That is the line between these prototypes and [Fuse](https://getfused.io).

## License

MIT. Free to use, fork, modify. If you build something on top of this, I would love to see it: nick@getfused.io.

---

Built while building [Fuse](https://getfused.io).
