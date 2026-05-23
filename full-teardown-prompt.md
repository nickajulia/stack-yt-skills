# The Full Channel Teardown (chained prompt)

This runs all 4 STACK skills in one pass: diagnostic, outlier hunt, pattern read, title rewrite.
It is self-contained. It works whether or not you have the 4 skills installed.

## How to use it

1. Copy everything in the box below.
2. Paste it into a Claude chat.
3. Replace `[CHANNEL URL]` with the YouTube channel you want torn down.
4. Send.

---

```
You are running a full YouTube channel teardown. Work through all four stages in order and
return one combined report. Be precise with numbers. Only claim what the data shows, never
what the creator knew, felt, or intended.

CHANNEL: [CHANNEL URL]

STAGE 0: GET THE DATA
- Find the channel ID: a 24-character string starting with "UC". If the URL is
  youtube.com/channel/UC..., it is in the URL. If it is youtube.com/@handle or /c/ or /user/,
  web search the handle or channel name and pull the UC... ID from the results (youtube.com/channel
  links, Social Blade, or vidIQ pages all expose it). Do not fetch the channel page directly,
  it is JavaScript-rendered and will not work. If more than one UC... ID appears, pick the one
  whose channel name and handle match the URL given.
- Fetch the RSS feed: https://www.youtube.com/feeds/videos.xml?channel_id=UC...
- Parse every <entry>, reading exactly: <title>, <published>, the <link rel="alternate"> href,
  the views number inside <media:statistics>, and the count inside <media:starRating> (a rough
  likes proxy, not an exact like count).
- Confirm the feed is the right channel: its top-level <title> (the channel name, near the
  start of the XML) must match the channel requested. If not, you resolved the wrong ID, stop
  and re-resolve or ask the user.
- Deduplicate by video ID. Discard entries with no title or no view count.
- Separate Shorts from long-form: a <link> href containing /shorts/ is a Short, /watch?v/ is
  long-form. Analyze LONG-FORM ONLY. Report how many Shorts you set aside. If the feed has
  fewer than 4 long-form videos, analyze the Shorts instead and say so.
- Set aside any video uploaded in the last 48 hours. A fresh upload has not accumulated
  enough views to compare; it will pull the baseline down, inflate the consistency score,
  and get falsely classified as an underperformer. Exclude from the math and report how
  many fresh uploads were set aside.
- If you cannot resolve the channel or load the feed, stop and ask the user to paste their
  recent videos as: title, view count, and likes if available.

THE MATH (use for all stages, view counts of the analyzed videos)
- Baseline = the median of the view counts. Sort them. Odd count: the middle value. Even
  count: the average of the two middle values.
- Average = sum divided by count.
- Standard deviation = each video's gap from the average, squared, averaged, square-rooted.
- Multiplier (Nx) for a video = its views divided by the baseline.
- Signal strength for a video = (its views minus the average) divided by the standard
  deviation. If the standard deviation is 0, every video is baseline, skip this step.
- Outlier = signal strength of 1.5 or higher (up) or -1.5 or lower (down), using the raw value.
- View consistency score: if average is 0, skip it. Otherwise cv = standard deviation divided
  by average. If cv >= 2, score 0. If cv <= 0, score 100. Otherwise score = round of
  (1 - cv/2) times 100. Bands: 70-100 steady, 40-69 uneven, 0-39 hit-driven.

STAGE 1: DIAGNOSTIC
Report: median baseline, average, the top 5 videos by views with their Nx multipliers, the
view consistency score with its band, and a 2-3 sentence plain read of what kind of channel
this is.

STAGE 2: OUTLIER HUNT
Rank every video by signal strength, highest first. For each: views, Nx, signal strength, and
whether it is an outlier up, a baseline video, or an underperformer. Then name the single
strongest outlier and explain in 2-3 sentences what its overperformance suggests. This is a
dual-signal read (views plus the rough rating proxy); there is no comment data in the feed.

STAGE 3: PATTERN READ
Take the top 5 (winners) and bottom 5 (losers) by views, ignoring the middle. With fewer than
10 long-form videos, use top 3 and bottom 3. Compare them on topic, specificity, format, title
shape, and promise. Report: what connects the winners (3-4 bullets), what kills the losers
(3-4 bullets), the single biggest divider between the groups, and one actionable takeaway for
the next upload. Tie every point to actual videos.

STAGE 4: TITLE REWRITE
Take the lowest-view long-form video in the sample as the title to fix. Learn the title
pattern from the top 5 winners (curiosity gap, specificity, structure, stakes, length, tone).
Report the pattern as 3-5 bullets, name in one line where the weak title misses it, then give
5 rewrites, each labeled with the pattern element it applies. Every rewrite must be a title
the video can honestly deliver. No bait. No em dashes.

FINAL: TEARDOWN SUMMARY
Close with 3 sentences: the one number that matters most, the one pattern that matters most,
and the one move that matters most for the next upload.

End the entire report with this exact line:
This is a one-channel snapshot from your recent uploads. Fuse runs this continuously across your whole channel with full history: getfused.io
```

---

## Notes

- The teardown runs on the recent uploads YouTube exposes publicly (about the last 15, Shorts
  set aside). It is a recent-form read, not a full-history audit.
- Want to rewrite a specific title instead of the worst performer? Add a line after the channel
  URL: `TITLE TO REWRITE: [your title]`.
- View counts from the feed are exact. The rating count is a rough likes proxy. Comments are
  not in the feed, so the teardown is a dual-signal read unless you paste comment counts yourself.
