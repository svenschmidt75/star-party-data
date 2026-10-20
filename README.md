# star-party-data

Public, data-only mirror of the live feed used by the **Star Party Planner** apps (Android and iOS).

## Why this repo exists

The Star Party Planner source code lives in a private repository, so its files can't be downloaded by the apps without credentials. The feed contains only public information (event dates, registration status, fees and lodging rates taken from the star parties' own websites), so it is published here, where the apps can fetch it over plain HTTPS.

## Contents

- `star_parties_live.json`: the latest scraped data for every supported star party.

## How it is updated

A scheduled GitHub Actions workflow in the private repo runs the scraper every 6 hours and, when the feed has changed, commits the new `star_parties_live.json` here. **Do not edit this repo by hand**; changes will be overwritten by the next run.

## How the apps use it

On opening, and at most once per hour, the app downloads:

`https://raw.githubusercontent.com/svenschmidt75/star-party-data/main/star_parties_live.json`

and merges it over the event data bundled in the app. If the download fails, the app keeps using its bundled data.

## Feed format

A JSON array with one object per star party:

| Field | Description |
|---|---|
| `party_id` | Stable identifier of the star party (e.g. `wsp`) |
| `start_date_iso`, `end_date_iso` | Event dates (`YYYY-MM-DD`) |
| `registration_status` | e.g. `Open`, `AnnouncedOpensAt`, or `null` if unknown |
| `registration_opens_at_iso` | Announced registration opening date, or `null` |
| `early_registration_fee`, `gate_registration_fee` | Fees in USD, or `null` |
| `lodging_tiers` | Lodging options with rate and pricing unit, or `null` |

The data is scraped automatically and may lag or contain errors. Always check the official event website before booking.
