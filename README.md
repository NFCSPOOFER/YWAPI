# YWAPI

YWAPI is a community-maintained, read-only API for Yo-kai Watch NFC collectibles.

The first supported section is **Yo-kai Arks**. The repo is structured to add Dream Medals, Treasure Medals / T Medals, Yo-seiken, Yo-kai Y Medals, and Genju Disks later.

This project is inspired by the simple static-data style of AmiiboAPI: apps should be able to fetch a small JSON file, match a scanned item, and display the correct name and images.

## Quick Start

All Arks:

```text
https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/api/ark/index.json
```

Lookup by decoded NFC identity:

```text
https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/api/ark/by-display-code/0103MBW.json
https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/api/ark/by-ark-key/MBW.json
https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/api/ark/by-numeric-id/28940.json
```

Lookup by API ID:

```text
https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/api/ark/by-id/YW-ARK-0338.json
```

## App Matching

When an app scans and decodes a Yo-kai Ark, match in this order:

1. `displayCode`
2. `arkKey`
3. `numericId`
4. manual fallback

UIDs are not used as the public item identity because duplicate physical tags of the same Ark have different UIDs.

## Object Example

```json
{
  "id": "YW-ARK-0338",
  "legacyId": "YKW-ARK-0338",
  "name": "Wildfire Blaze",
  "series": "Sacred Armory",
  "seriesGroup": "Exclusive/Promotional Keystones",
  "rarity": null,
  "type": "Ark",
  "catalogNumber": 338,
  "sourceNumber": 6,
  "displayCode": "0103MBW",
  "displayCodes": ["0103MBW"],
  "arkKey": "MBW",
  "arkKeys": ["MBW"],
  "numericId": 28940,
  "numericIds": [28940],
  "image": "https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/images/ark/front/YW-ARK-0338.png",
  "imageBack": "https://raw.githubusercontent.com/ibrahimnalzaabi/YWAPI/main/images/ark/back/YW-ARK-0338.png",
  "status": "confirmed",
  "scanCount": 2,
  "uniqueTagCount": 2
}
```

## Status Values

- `confirmed`: multiple community scans support the identity.
- `single_scan`: one successful community scan supports the identity.
- `missing`: no successful decoded scan yet.
- `conflict`: multiple decoded identities currently exist for the catalog entry.

## Docs

See [docs/API.md](docs/API.md) for endpoint details.

## Disclaimer

YWAPI is an unofficial community preservation project and is not affiliated with Bandai, Level-5, Nintendo, or any other rights holder.
