---
name: gpt-image-gen
description: Generate an image via the OpenAI Images API (gpt-image-2) from a finalized prompt. Use when an agent has a ready-to-send image prompt and needs a PNG file written to disk. Reads OPENAI_API_KEY from .env. The skill does not author prompts — it only calls the API and saves the result.
---

# gpt-image-gen

מעטפת דקה ל-OpenAI Images API. **לא** משכתב prompts ולא בוחר נושא — מקבל prompt מוכן וקורא ל-API. כל סוכן שצריך להפיק תמונה משתמש בסקיל הזה.

## מה הסקיל עושה

מקבל prompt + נתיב יעד, שולח קריאה ל-`POST https://api.openai.com/v1/images/generations`, ושומר את התמונה (PNG) לדיסק. זהו.

## Inputs (חובה לספק לפני קריאה)

| משתנה | תיאור | ברירת מחדל |
|---|---|---|
| `PROMPT` | טקסט ה-prompt באנגלית | — (חובה) |
| `OUT` | absolute path לקובץ ה-PNG היעד | — (חובה) |
| `SIZE` | `1024x1024` / `1536x1024` / `1024x1536` | `1024x1024` |
| `QUALITY` | `low` / `medium` / `high` | `medium` |

הסקיל לא קובע איפה הפלט נשמר — הקורא (יובל או סוכן אחר) קובע. ה-caller צריך לייצא אותם כ-environment variables לפני הרצת הסקריפט.

## Pre-flight (חובה)

1. ודא ש-`OPENAI_API_KEY` קיים ולא ריק:
   ```bash
   [ -n "$OPENAI_API_KEY" ] || { echo "OPENAI_API_KEY missing"; exit 1; }
   ```
   המפתח נטען מ-`.env` בשורש הפרויקט. אם ריק — **fail loud, אל תקרא ל-API**.
2. ודא שתיקיית היעד קיימת:
   ```bash
   mkdir -p "$(dirname "$OUT")"
   ```

## Path A — `jq` זמין (Linux/macOS)

```bash
curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n --arg p "$PROMPT" --arg s "${SIZE:-1024x1024}" --arg q "${QUALITY:-medium}" \
        '{model:"gpt-image-2",prompt:$p,size:$s,quality:$q,output_format:"png"}')" \
  | tee /tmp/gpt-image-response.json \
  | jq -r '.data[0].b64_json // empty' \
  | base64 --decode > "$OUT"

if [ ! -s "$OUT" ]; then
  echo "ERROR: image not created. API response:"
  cat /tmp/gpt-image-response.json
  exit 1
fi
```

## Path B — Python fallback (ברירת מחדל בפרויקט הזה)

הסביבה של הפרויקט היא Git Bash על Windows — **`jq` לא מותקן**. השתמש בנתיב הזה כברירת מחדל.

```bash
PROMPT="$PROMPT" OUT="$OUT" SIZE="${SIZE:-1024x1024}" QUALITY="${QUALITY:-medium}" \
OPENAI_API_KEY="$OPENAI_API_KEY" python - <<'PY'
import os, json, base64, urllib.request, urllib.error, sys

prompt  = os.environ["PROMPT"]
out     = os.environ["OUT"]
size    = os.environ.get("SIZE", "1024x1024")
quality = os.environ.get("QUALITY", "medium")
key     = os.environ["OPENAI_API_KEY"]

body = json.dumps({
    "model": "gpt-image-2",
    "prompt": prompt,
    "size": size,
    "quality": quality,
    "output_format": "png",
}).encode()

req = urllib.request.Request(
    "https://api.openai.com/v1/images/generations",
    data=body,
    headers={
        "Authorization": f"Bearer {key}",
        "Content-Type": "application/json",
    },
)

try:
    with urllib.request.urlopen(req) as r:
        data = json.load(r)
except urllib.error.HTTPError as e:
    err_body = e.read().decode("utf-8", errors="replace")
    print(f"HTTPError {e.code}: {err_body}", file=sys.stderr)
    sys.exit(1)

if "data" not in data or not data["data"]:
    msg = data.get("error", {}).get("message", json.dumps(data))
    print(f"API error: {msg}", file=sys.stderr)
    sys.exit(1)

b64 = data["data"][0].get("b64_json")
if not b64:
    print(f"No b64_json in response: {json.dumps(data)[:500]}", file=sys.stderr)
    sys.exit(1)

with open(out, "wb") as f:
    f.write(base64.b64decode(b64))
print(out)
PY
```

## Post-flight (חובה)

```bash
[ -s "$OUT" ] || { echo "ERROR: $OUT is missing or empty"; exit 1; }
echo "Saved: $OUT ($(wc -c < "$OUT") bytes)"
```

## טעינת `.env` (אם המשתנים לא ב-shell environment)

אם הקורא לסקיל רץ בלי שטען קודם את `.env`:
```bash
set -a
. .env
set +a
```
הריץ פעם אחת בתחילת ה-session, אחרי `cd` לשורש הפרויקט.

## Error handling — סיכום

| מצב | תגובה |
|---|---|
| `OPENAI_API_KEY` ריק | exit 1 לפני קריאה ל-API |
| HTTPError (4xx/5xx) | הדפס את גוף התגובה, exit 1 |
| תגובה ללא `data` (יש `error`) | הדפס `error.message`, exit 1 |
| `b64_json` חסר | הדפס את 500 התווים הראשונים מהתגובה, exit 1 |
| קובץ פלט 0 בייט / לא קיים | exit 1 עם הודעה |

## דוגמה לקריאה מלאה (כפי שיובל מריץ)

```bash
PROMPT="A minimalist illustration of a cat reading a book, soft pastel palette, clean vector lines, neutral background"
OUT="yuval/outputs/2026-05-06-cat-reading-book.png"

[ -n "$OPENAI_API_KEY" ] || { echo "OPENAI_API_KEY missing"; exit 1; }
mkdir -p "$(dirname "$OUT")"

# (הרץ את בלוק Python של Path B כאן)

[ -s "$OUT" ] && echo "OK: $OUT"
```

הסקיל לא יוצר את קובץ ה-`.txt` עם ה-prompt — זה התפקיד של הסוכן הקורא (יובל).
