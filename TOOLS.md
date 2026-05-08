# TOOLS.md — Heidi's Technical Reference

_Resource IDs, endpoints, scripts, and configuration. Update this when anything changes._

---

## Google Authentication

- **Token path:** `~/.openclaw/workspace-heidi/google_token.json`
- **Scopes:** Gmail (read + send), Calendar (read + write), Sheets, Docs, Drive

---

## Google Calendars

| Calendar | ID | Access |
|----------|----|--------|
| 🌻 Heidi (Heidi's write space) | `[EMAIL REDACTED]` | Read + Write |
| [OWNER] Personal | `[OWNER EMAIL]` | Read + Write |
| Couple Calendar | `[EMAIL REDACTED]` | Read + Write |
| [FAMILY NAME] Babies | `[CALENDAR ID]` | **Read only** |

**OOO events on Heidi calendar:** always add `[OWNER WORK EMAIL]` as guest with `sendUpdates=all`

---

## Google Docs & Sheets

| Resource | ID | Purpose |
|----------|----|---------|
| Sage ↔ Heidi Meal Planning Doc | `[DOC ID]` | Sage writes health notes by Saturday EOD; Heidi reads for meal planning |
| Returns tracking list | `🌻 Heidi - Returns` list in Apple Reminders | Log return items; surface Saturdays + urgent nudge within 3 days |
| Grocery list | Shared **Groceries** list in Apple Reminders | Add items via osascript: `osascript -e 'tell application "Reminders" to make new reminder at list "Groceries" with properties {name: "ITEM"}'` |

---

## Email

| Field | Value |
|-------|-------|
| Send from | `[AGENT EMAIL]` |
| CC on every email | `[OWNER EMAIL]` |
| Identity | Always sign as Heidi, never impersonate [OWNER] |
| Send script | `scripts/send_egg_invoice.py` (Gmail API via google_token.json) |

---

## Contacts

| Person | Email | Notes |
|--------|-------|-------|
| [OWNER] | [OWNER EMAIL] | Primary |
| [OWNER] (work) | [OWNER WORK EMAIL] | Never contact without explicit per-instance permission |
| [PARTNER] | [PARTNER EMAIL] | CC for family emails |
| [FAMILY MEMBER] | [FAMILY EMAIL] | CC for [FAMILY MEMBER]-directed emails |
| Whitney (Two Sparrows Farm) | [EMAIL REDACTED] | Egg invoice recipient — credits [OWNER]'s account |

---

## Weather API

**Source:** Open-Meteo (no API key required)
**Location:** [LOCATION] · lat `[LAT]` · lon `[LON]`

2-day (morning brief):
```
curl "https://api.open-meteo.com/v1/forecast?latitude=[LAT]&longitude=[LON]&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,weathercode&hourly=temperature_2m,precipitation_probability,weathercode&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&timezone=America%2FDetroit&forecast_days=2"
```

7-day (outfit planning, weekly brief):
```
curl "https://api.open-meteo.com/v1/forecast?latitude=[LAT]&longitude=[LON]&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,weathercode,windspeed_10m_max&hourly=temperature_2m,precipitation_probability,weathercode&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&timezone=America%2FDetroit&forecast_days=7"
```

---

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/send_egg_invoice.py` | Generate and send monthly egg invoice via Gmail API |
| `scripts/google_auth.py` | Re-authenticate Google token if it expires |
| `scripts/test_google.py` | Test Google API connection |

---

## Egg Sales

| Item | Unit Price |
|------|-----------|
| Chicken eggs | $4.00 / dozen |
| Duck eggs | $4.00 / half dozen |

- Log: `eggs/egg_log.md`
- Invoice recipient: Whitney at [EMAIL REDACTED] (CC [OWNER EMAIL])
- Always CC [OWNER EMAIL] on invoice emails
