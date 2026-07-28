# YouTube Watch History — Viewing Habits Analysis

**Author:** Ohanzee Karneh **Tools:** Python (Pandas, Matplotlib) | Jupyter Notebook **Data Source:** Personal YouTube data exported via [Google Takeout](https://takeout.google.com/) (JSON + CSV)

---

## Project Overview

This project explores 17 months of personal YouTube watch history (Feb 2025 – Jul 2026) to uncover viewing patterns, subscription loyalty, and content discovery habits. The raw data was cleaned and transformed using Pandas, then analyzed and visualized in a Jupyter Notebook to answer questions about how I consume content on the platform.

---

## Data Cleaning Pipeline (Pandas)

1. **JSON Parsing** — Loaded 42,200 records from nested JSON using `json_normalize`; extracted channel names from nested `subtitles` field
2. **Ad Removal** — Filtered out 8,600+ Google Ads entries using the `details` field
3. **Non-Video Filter** — Removed "Visited," "Used," and "Viewed" entries to isolate actual video watches
4. **Prefix Stripping** — Removed the "Watched " prefix from all video titles
5. **Datetime Conversion** — Parsed UTC timestamps to proper datetime objects
6. **Subscription Join** — Merged subscription data (1,428 channels) to create a boolean `subscribed` column
7. **Final Dataset** — 26,200 clean video watch records with 5 columns

---

## Questions & Findings

### Q1: What percentage of my watched videos are from subscriptions?

**Methodology:** Summed the number of rows within the subscription column and divided it by the number of rows in the dataset (`sum()` and `len()`). I then converted that value into a percentage for printing.

**Finding:** Only 21.8% of videos watched came from subscribed channels.

**Insight:** Most of the content I watch on YouTube is from my recommended feed and random searches instead of current subscriptions.

---

### Q2: Who are my top 10 subscribed channels and top 10 unsubscribed channels?

**Methodology:** Created two datasets consisting of subscription videos and non-subscription videos (`df[df['subscribed'] == True/False]['channelName']`). I then used `.value_counts().head(10)` to obtain the top 10 channels from each group before creating my bar chart.

**Visual:** Horizontal bar charts (side-by-side comparison)

**Key Finding:** Shorts inflate watch counts — my top 2 subscriptions primarily make short-form content. My top 3 long-form creators would be RabSoPetty, Dmolition, and YaBoyRoshi.

---

### Q3: From my subscriptions, what is the average number of videos I've seen, the highest, and the lowest?

**Methodology:** Reused the previous subscribed group from question 2, then computed descriptive statistics (`.mean(), .max(), .min()`).

**Key Finding:** Mean watches per subscription: ~10. Median: 3 — indicating heavy right-skew from a few binge channels.

**Insight:** I tend not to be loyal to any singular channel but will subscribe if I like even one of a channel's videos.

---

### Q4A: What is the average number of videos I watch per day and per month?

**Methodology:** Created groups by day and month using `time.dt.date` and `time.dt.to_period('M')` and then computed their means.

**Insight:** I consume a lot more YouTube content than I had initially anticipated when starting this project, even with shorts being included alongside long-form video.

---

### Q4B: Which months did I watch the most videos?

**Methodology:** I reused the month group from Q4A to plot a line chart using `.size().plot`

**Visual:** Line chart — monthly video counts across the full date range.

**Insight:** In June and July of 2025, I was working a lot of hours and went to Africa for a month, explaining the sharp depression after May. During the early semester and on breaks, I watch a lot of content, but there's a noticeable drop around finals.

---

### Q5: Between subscribed and unsubscribed channels, which have the longest title length on average?

**Methodology:** Added a `titleLength` column using `str.len()` to the cleaned dataset and then plotted overlapping histograms split by subscription status.

**Visual:** Overlapping histogram (alpha transparency, 20 bins)

**Insight:** The content recommended to me (unsubscribed) typically has a title length of around 40 characters, implying that there's a sweet spot in YouTube's algorithm for recommended content.

---

### Q6: What proportion of my subscriptions are above average watches vs. below?

**Methodology:** Created a new dataset with only my subscriptions and dropped that column (`.drop`). I then computed the mean (10.28 from `.mean()`) and median (3.0 `.median()`) watches for each subscribed channel. The large gap confirmed a right-skew, so the median was used to anchor category boundaries:

| Category | Range |
| --- | --- |
| Dead | 0–3 watches |
| Low | 4–9 watches |
| Moderate | 10–29 watches |
| High | 30+ watches |

**Visual:** Pie chart — Subscription Activity Breakdown

**Key Finding:** 58.5% of subscriptions fell in the "dead" tier (≤3 watches). Only 8% had 30+ watches.

**Insight:** There is a skew on the mean number of watches per subscription. The median was a more accurate depiction of where most of my subscriptions fall. I normally watch fewer than 4 videos per subscription, and only 8% of them have 30+ watches.

---

### Q7: How has my proportion of subscribed/unsubscribed watching changed over time?

**Methodology:** Grouped by month and subscription status using `.groupby()` + `.unstack()`, then plotted as a stacked area chart.

**Visual:** Stacked area chart — subscribed vs. unsubscribed viewing by month

**Insight:** I have been watching more and more videos straight from my recommended feed rather than from my subscriptions.

---

## Key Takeaways

1. **YouTube's recommendation engine drives ~75% of my viewing** — subscriptions are more of a passive follow than an active content source.
2. **58.5% of subscriptions are essentially dead** (≤3 watches in 17 months) — most subscribes are impulsive, not intentional.
3. **Short-form content inflates channel rankings** — raw watch counts don't reflect actual engagement with long-form creators.
4. **Viewing habits correlate with life events** — work schedules, travel, and academic calendars all leave clear fingerprints in the data.
5. **The trend is toward more algorithm-driven consumption** — subscribed viewing is shrinking as a proportion over time.

---

## Limitations

1. **No watch duration data** — YouTube's export only provides binary "watched" events, not how long each video was viewed. Shorts and 2-hour videos are weighted equally.
2. **Subscription timing unknown** — The subscription export is a snapshot of current subscriptions. Channels unsubscribed before the export are not captured.
3. **Name matching imperfections** — Some channels may have changed names between the watch event and the subscription export, causing false negatives in the `subscribed` flag.
4. **UTC timestamps** — All times are in UTC; hour-of-day analysis may be slightly offset from actual Eastern Time viewing.

---

## Tools & Technologies

| Tool | Purpose |
| --- | --- |
| **Python** | Data cleaning, transformation, and analysis |
| **Pandas** | DataFrame manipulation, groupby aggregations, merging |
| **Matplotlib** | Bar charts, histograms, line charts, pie charts, area charts |
| **Jupyter Notebook** | Development environment for cleaning and analysis |
| **Google Takeout** | Data export from YouTube (JSON + CSV) |

---

## Repository Contents

| File | Description |
| --- | --- |
| `Youtube Cleaning.ipynb` | Pandas cleaning and transformation notebook |
| `Youtube Analysis And Visualization.ipynb` | Full analysis with visualizations |
| `youtube_clean.csv` | Final cleaned dataset |
| `subscriptions.csv` | Raw subscription data from Google Takeout |
| `watch-history.json` | Raw watch history from Google Takeout |
| `images/` | Visualization screenshots |
| `README.md` | Project overview and documentation |

