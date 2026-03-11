# MEMORY: COMBINER CARRIE

## Analytical Notes

- `latest_themes.json` always reflects the most recently completed run.
  If the file does not exist (first ever run), skip sentiment comparison and
  set `sentiment_changed: false`, `previous_sentiment: null` for all themes.
- When matching themes across runs, prioritize topic identity over exact
  name match. "GPT-5 Speculation" and "GPT-5 Release Timeline" likely
  represent the same ongoing theme.
- When a theme is genuinely new (no prior run match), record it as new with
  no sentiment change note.

## Recurring Theme Reference

Use consistent names when the same theme recurs to enable accurate
sentiment tracking over time. Record established theme names below.

<!-- Combiner Carrie updates this table as stable recurring themes emerge -->

| Established Theme Name              | First Seen  | Notes                        |
|-------------------------------------|-------------|------------------------------|
| —                                   | —           | —                            |
