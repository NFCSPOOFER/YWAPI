# YWAPI Documentation

## Information

YWAPI is a read-only, static JSON API for Yo-kai Watch NFC collectibles.

No authentication is required. All endpoints are plain JSON files and can be cached by apps.

Base URL:

```text
https://raw.githubusercontent.com/ibrahimalzaabi/YWAPI/main
```

## Ark

Returns Yo-kai Ark information.

### All Arks

```text
GET /api/ark/index.json
```

Response:

```json
{
  "ark": [
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
      "image": "https://raw.githubusercontent.com/ibrahimalzaabi/YWAPI/main/images/ark/front/YW-ARK-0338.png",
      "imageBack": "https://raw.githubusercontent.com/ibrahimalzaabi/YWAPI/main/images/ark/back/YW-ARK-0338.png",
      "status": "confirmed",
      "scanCount": 2,
      "uniqueTagCount": 2
    }
  ]
}
```

### By ID

```text
GET /api/ark/by-id/YW-ARK-0338.json
```

### By Legacy ID

```text
GET /api/ark/by-legacy-id/YKW-ARK-0338.json
```

### By Catalog Number

Catalog numbers are zero-padded to four digits.

```text
GET /api/ark/by-number/0338.json
```

### By Display Code

Recommended lookup for apps after scanning and decoding an Ark.

```text
GET /api/ark/by-display-code/0103MBW.json
```

### By Ark Key

Fallback lookup if a display code is unavailable.

```text
GET /api/ark/by-ark-key/MBW.json
```

### By Numeric ID

Fallback lookup if only the decoded numeric ID is available.

```text
GET /api/ark/by-numeric-id/28940.json
```

### By Series

Series names are slugged.

```text
GET /api/ark/by-series/sacred-armory.json
```

### By Status

```text
GET /api/ark/by-status/confirmed.json
GET /api/ark/by-status/single-scan.json
GET /api/ark/by-status/missing.json
GET /api/ark/by-status/conflict.json
```

## Type

Lists supported Yo-kai Watch NFC item sections.

```text
GET /api/type/index.json
```

## Series

Lists known Ark series.

```text
GET /api/series/index.json
```

## Status

Lists status values and counts.

```text
GET /api/status/index.json
```

## Last Updated

```text
GET /api/lastupdated/index.json
```

Response:

```json
{
  "lastUpdated": "2026-08-25T00:00:00+00:00"
}
```

## Field Reference

| Name | Description | Type |
| --- | --- | --- |
| `id` | Public YWAPI ID. | string |
| `legacyId` | Original internal project ID. | string |
| `name` | Item name. | string |
| `series` | Specific product/set series. | string or null |
| `seriesGroup` | Larger series grouping. | string or null |
| `rarity` | Rarity or release label, when known. | string or null |
| `type` | Item type. Currently `Ark`. | string |
| `catalogNumber` | Catalog number. | number |
| `sourceNumber` | Number from the source spreadsheet/list, when present. | number or null |
| `displayCode` | Primary decoded display code for app matching. | string or null |
| `displayCodes` | All known decoded display codes for this entry. | string[] |
| `arkKey` | Primary decoded Ark key for app matching. | string or null |
| `arkKeys` | All known decoded Ark keys for this entry. | string[] |
| `numericId` | Primary decoded numeric ID. | number or null |
| `numericIds` | All known decoded numeric IDs for this entry. | number[] |
| `image` | Front PNG URL. | string or null |
| `imageBack` | Back PNG URL. | string or null |
| `status` | `confirmed`, `single_scan`, `missing`, or `conflict`. | string |
| `scanCount` | Successful scan count in the community dataset. | number |
| `uniqueTagCount` | Unique physical UID count in the community dataset. | number |

## App Lookup Order

Apps should match scanned Ark data in this order:

1. `displayCode`
2. `arkKey`
3. `numericId`
4. manual search or manual selection

Do not use UID as the public item identity. UID identifies the physical tag, not the Ark character/item.
