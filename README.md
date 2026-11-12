# Codex Resets

[![Codex reset](https://codexreset.in/badge.svg)](https://codexreset.in)

**Know the moment Codex usage limits reset.** [codexreset.in](https://codexreset.in) tracks every regular and banked Codex reset, taken straight from Tibo’s ([@thsottiaux](https://x.com/thsottiaux)) public posts on X and checked against the source.

This repo is the public home for the free **README badge**, **RSS feed** and **JSON API**. Everything here works without a key or an account.

[![Share card for a banked Codex reset](https://codexreset.in/og/reset/2026-11-01-2.png)](https://codexreset.in/reset/2026-11-01-2)

---

## README badge

Show the latest reset in any README, or what’s coming when Tibo drops a hint.

```markdown
[![Codex reset](https://codexreset.in/badge.svg)](https://codexreset.in)
```

```html
<a href="https://codexreset.in"><img src="https://codexreset.in/badge.svg" alt="Codex reset status"></a>
```

The badge has three states:

| Badge says | Means | Colour |
| --- | --- | --- |
| `banked · 3h ago` | The latest reset and how long ago it was | Green within a day, orange after |
| `banked likely by Thu` | Tibo has hinted at a reset that hasn’t landed yet | Purple |
| `no data yet` | Nothing recorded | Grey |

It refreshes every few minutes. GitHub caches images briefly, so a new reset can take a few minutes to appear in your README.

---

## RSS feed

```
https://codexreset.in/feed.xml
```

The latest 50 resets, newest first, each with the original post and a link to its page. Works in Feedly, Inoreader, Slack’s RSS app, Discord feed bots and anything else that reads RSS.

---

## JSON API

No key, no sign-up. CORS is open, so it works straight from a browser. Responses are cached for two minutes, so polling more often than that gains nothing.

| Endpoint | Returns |
| --- | --- |
| `GET https://codexreset.in/api/v1/status` | The latest reset, any active watch, and totals |
| `GET https://codexreset.in/api/v1/resets` | Every reset, newest first. Optional `?type=regular` or `?type=banked`, and `?limit=` from 1 to 500 (default 100) |
| `GET https://codexreset.in/feed.xml` | RSS 2.0, latest 50 resets |
| `GET https://codexreset.in/badge.svg` | The README badge |
| `GET https://codexreset.in/og/reset/{slug}.png` | The 1200×630 share image for one reset |

### `GET /api/v1/status`

```bash
curl https://codexreset.in/api/v1/status
```

```json
{
  "last_reset": {
    "id": "2117035794678546764",
    "slug": "2026-11-01-2",
    "type": "banked",
    "announced_at": "2026-11-01T23:27:20.000Z",
    "page_url": "https://codexreset.in/reset/2026-11-01-2",
    "post_url": "https://x.com/thsottiaux/status/2117035794678546764",
    "image_url": "https://codexreset.in/og/reset/2026-11-01-2.png",
    "text": "Day 28/ Banked 2/2",
    "seconds_ago": 902982
  },
  "watch": {
    "type": "banked",
    "window": "by end of Thursday (PT)",
    "expires_at": "2026-11-13T08:00:00.000Z",
    "observed_at": "2026-11-11T06:57:08.000Z",
    "post_url": "https://x.com/thsottiaux/status/2120410480204525936",
    "text": "Thursday is feeling codexy to me. Perhaps also a good time for a banked reset."
  },
  "totals": { "resets": 60, "regular": 50, "banked": 10 },
  "source": "https://x.com/thsottiaux",
  "updated_at": "2026-11-12T10:17:01.479Z",
  "docs": "https://codexreset.in/developers"
}
```

`watch` is `null` when there’s no active hint. `last_reset` is `null` only if nothing has been recorded.

### `GET /api/v1/resets`

```bash
curl "https://codexreset.in/api/v1/resets?type=banked&limit=1"
```

```json
{
  "resets": [
    {
      "id": "2117035794678546764",
      "slug": "2026-11-01-2",
      "type": "banked",
      "announced_at": "2026-11-01T23:27:20.000Z",
      "page_url": "https://codexreset.in/reset/2026-11-01-2",
      "post_url": "https://x.com/thsottiaux/status/2117035794678546764",
      "image_url": "https://codexreset.in/og/reset/2026-11-01-2.png",
      "text": "Day 28/ Banked 2/2"
    }
  ],
  "count": 1,
  "total": 10,
  "docs": "https://codexreset.in/developers"
}
```

### Fields

| Field | Meaning |
| --- | --- |
| `type` | `regular` (applied to your limits for you) or `banked` (saved to your account; redeem it yourself from `/usage` in Codex) |
| `slug` | The reset’s UTC date, with `-2`, `-3`… when a day has more than one. Its page is `https://codexreset.in/reset/{slug}` |
| `announced_at` | When the post went up, ISO 8601 in UTC |
| `post_url` | The original post on X |
| `text` | The post’s text, links removed |

---

## Examples

**Shell: the latest reset in one line**

```bash
curl -s https://codexreset.in/api/v1/status | jq -r '.last_reset | "\(.type) reset, \(.announced_at)"'
```

**JavaScript: show a notice when a hint is active**

```js
const status = await fetch('https://codexreset.in/api/v1/status').then(r => r.json());
if (status.watch) console.log(`A ${status.watch.type} reset may land ${status.watch.window}.`);
else console.log(`Last reset: ${status.last_reset.type}, ${status.last_reset.announced_at}`);
```

**Python: count banked resets**

```python
import json, urllib.request
data = json.load(urllib.request.urlopen("https://codexreset.in/api/v1/resets?type=banked&limit=500"))
print(f"{data['total']} banked resets on record")
```

---

## Fair use

Free for personal and commercial projects. A link back to [codexreset.in](https://codexreset.in) is appreciated. Please respect the two-minute cache rather than polling harder.

## FAQ

**Where does the data come from?** Tibo’s public posts on X. Each record is checked against his pinned account ID and handle, and links back to the original post. Hints and replies that need context are kept separate from confirmed resets. See [how validation works](https://codexreset.in/validation).

**How fast does a new reset show up?** The site checks X about every two minutes. The API, feed and badge follow within a few minutes after that.

**Is this official?** No. Codex Resets is an independent tracker and isn’t affiliated with OpenAI. For how resets work on your account, see [OpenAI’s help article](https://help.openai.com/articles/20001498).

**Want email alerts instead?** Sign up at [codexreset.in](https://codexreset.in). One email per reset, one-click unsubscribe.

**Found a problem or have an idea?** [Open an issue](../../issues) in this repo.

---

The site’s source code isn’t public; this repo holds only documentation.
