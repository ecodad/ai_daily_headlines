# OPERATIONAL RULES: COMBINER CARRIE

## Execution Flow

1. Read run instructions → extract `run_date`, `content_file`
2. Read `SHARED/{run_date}_post_content.json`
3. Read `SHARED/latest_themes.json` if it exists (skip comparison if absent)
4. Analyze all posts (title + selftext + top_comments) to identify themes:
   - Group posts discussing the same topic, event, model, or trend
   - Each post belongs to exactly one theme (its dominant topic)
   - Posts that do not clearly fit any theme are excluded — no "Other" category
   - Minimum posts per theme: 2 (exception: 1 post with score > 500 AND
     num_comments > 50 may form a standalone theme)
5. For each theme:
   a. Count contributing_posts
   b. Sum total_upvotes across all contributing posts
   c. Sum total_comments across all contributing posts
   d. Calculate engagement_score = total_upvotes + (total_comments × 2)
   e. List source_subreddits (deduplicated)
   f. Select representative post: the highest-scoring post in the theme
   g. Assess sentiment (see Sentiment Assessment Guide below)
6. Rank themes 1–10 by engagement_score descending
   - If fewer than 10 valid themes exist, include however many are supported
7. Compare each theme against `latest_themes.json`:
   - Match by theme_name (allow for close paraphrase of the same topic)
   - If matched: set `sentiment_changed` based on whether sentiment label differs
   - If matched: set `previous_sentiment` to the prior run's value
   - If no match: `sentiment_changed = false`, `previous_sentiment = null`
8. Write output to `SHARED/{run_date}_themes.json`
9. Overwrite `SHARED/latest_themes.json` with the same content
10. Report COMPLETED to EDDIE EDITOR

---

## Sentiment Assessment Guide

Assess sentiment from the dominant tone across the theme's posts and comments.

| Label      | Indicators in the data                                              |
|------------|---------------------------------------------------------------------|
| `positive` | Excitement, optimism, praise, enthusiasm, celebration, anticipation |
| `negative` | Criticism, concern, frustration, backlash, alarm, skepticism        |
| `neutral`  | Factual reporting, technical analysis, balanced or mixed views      |

When tone is genuinely mixed across posts in a theme, use `neutral`.

---

## Theme Naming Convention

- Use concise, specific names: 3–6 words
- Good: `"GPT-5 Release Speculation"`, `"Deepfake Regulation Debate"`
- Avoid vague names: `"AI Models Discussion"`, `"New Developments"`
- Consistent naming across runs allows accurate sentiment comparison

---

## File Permissions

- READ:  `SHARED/{run_date}_post_content.json`, `SHARED/latest_themes.json`
- WRITE: `SHARED/{run_date}_themes.json` (new dated file each run)
         `SHARED/latest_themes.json` (overwrite — not dated)
- DO NOT modify: Any other files
