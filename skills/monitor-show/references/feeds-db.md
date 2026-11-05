# The feeds database

Keep one SQLite file, such as `~/podbox/shows.db`, outside any git repository. Ask the user where if they have a preference.

## Create the tables

```sql
CREATE TABLE feed (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  type TEXT NOT NULL CHECK (type IN ('rss', 'youtube')),
  url TEXT NOT NULL,
  last_poll TEXT
);

CREATE TABLE episode (
  feed_id TEXT NOT NULL REFERENCES feed (id),
  guid TEXT NOT NULL,
  title TEXT,
  published TEXT,
  audio_url TEXT,
  audio_bytes INTEGER,
  episode_id TEXT,
  state TEXT NOT NULL DEFAULT 'new' CHECK (state IN ('new', 'skipped', 'done')),
  PRIMARY KEY (feed_id, guid)
);
```

`feed.id` is a short, stable name you choose, such as `city-desk`.

`type` is `rss` or `youtube`. For `rss`, `url` is the feed URL. For `youtube`, it is the channel, playlist, or page URL.

Store times as UTC `YYYY-MM-DDTHH:MM:SSZ`. `last_poll` is the time of the last poll, or null before the first.

`guid` is how you recognize the same item on the next poll. For `rss`, use the item's `<guid>`. If the item has none, use the enclosure URL. For `youtube`, use the video id. The primary key makes a second insert of the same item fail, so use `INSERT OR IGNORE`.

`audio_url` and `audio_bytes` come from the RSS `<enclosure>` `url` and `length`. For `youtube`, leave both null and use the page URL.

`episode_id` is the `episodeId` in the episode's summary file: podcue's `ep_…` id, or the id you chose. The summary file holds no show, title, date or link, so this row is where they live.

`state` is `new` until the episode is handled, `skipped` when the user chose not to process it, and `done` after it is handled.

## Find a feed

When the user names a show but not a feed, search for its RSS feed. The show's site, Apple Podcasts, and podcast directories usually link it. Check that the URL returns RSS with `<item>` elements before you store it.

## Poll

Read every row in `feed`.

For `rss`, fetch the feed and read each `<item>`: `<guid>`, `<title>`, `<pubDate>`, and `<enclosure>`. Convert `<pubDate>` to UTC.

For `youtube`, list the recent videos. Ask the user before you install yt-dlp; `uvx yt-dlp` runs it without installing.

```
yt-dlp --flat-playlist --playlist-end 20 --print "%(id)s %(title)s" "<url>"
```

Then set `last_poll` to the current UTC time.

For each item, insert a row with `INSERT OR IGNORE`. The rows you just inserted are the new episodes.

An episode left in state `new` from an earlier poll did not finish. Pick it up again on this poll.
