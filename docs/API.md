# YWAPI Documentation

YWAPI is a read-only static JSON API for Yo-kai Watch NFC collectibles.

No authentication is required. All endpoints are regular files hosted from the GitHub repository and can be cached by apps.

## Base URL

```text
https://raw.githubusercontent.com/NFCSPOOFER/YWAPI/main
```

## Response Style

Collection endpoints return an array named after the section:

```json
{
  "ark": []
}
```

Single-item endpoints return one object:

```json
{
  "ark": {}
}
```

## Ark Endpoints

| Purpose | Request |
| --- | --- |
| All Arks | `GET /api/ark/index.json` |
| All Arks alias | `GET /api/ark/all.json` |
| CSV export | `GET /api/ark/ark.csv` |
| By YWAPI ID | `GET /api/ark/by-id/YW-ARK-0338.json` |
| By legacy ID | `GET /api/ark/by-legacy-id/YKW-ARK-0338.json` |
| By catalog number | `GET /api/ark/by-number/0338.json` |
| By display code | `GET /api/ark/by-display-code/0103MBW.json` |
| By Ark key | `GET /api/ark/by-ark-key/MBW.json` |
| By numeric ID | `GET /api/ark/by-numeric-id/28940.json` |
| By series | `GET /api/ark/by-series/sacred-armory.json` |
| By status | `GET /api/ark/by-status/confirmed.json` |

## Index Endpoints

| Purpose | Request |
| --- | --- |
| API summary | `GET /api/index.json` |
| Supported item types | `GET /api/type/index.json` |
| Known Ark series | `GET /api/series/index.json` |
| Status values and counts | `GET /api/status/index.json` |
| Last generated timestamp | `GET /api/lastupdated/index.json` |

## Field Reference

| Name | Type | Description |
| --- | --- | --- |
| `id` | string | Public YWAPI ID. |
| `legacyId` | string | Original internal project ID retained for compatibility. |
| `name` | string | Item name. |
| `series` | string or null | Specific product/set series. |
| `seriesGroup` | string or null | Larger series grouping. |
| `rarity` | string or null | Rarity or release label, when known. |
| `type` | string | Item type. Currently `Ark`. |
| `catalogNumber` | number | Catalog number. |
| `sourceNumber` | number or null | Number from the source spreadsheet/list, when present. |
| `displayCode` | string or null | Primary decoded display code for app matching. |
| `displayCodes` | string[] | All known decoded display codes for this entry. |
| `arkKey` | string or null | Primary decoded Ark key for app matching. |
| `arkKeys` | string[] | All known decoded Ark keys for this entry. |
| `numericId` | number or null | Primary decoded numeric ID. |
| `numericIds` | number[] | All known decoded numeric IDs for this entry. |
| `image` | string or null | Front PNG URL. |
| `imageBack` | string or null | Back PNG URL. |
| `status` | string | `confirmed`, `single_scan`, `missing`, or `conflict`. |
| `scanCount` | number | Successful scan count in the community dataset. |
| `uniqueTagCount` | number | Unique physical UID count in the community dataset. |

## App Lookup Order

Apps should match scanned Ark data in this order:

1. `displayCode`
2. `arkKey`
3. `numericId`
4. manual search or manual selection

Do not use UID as the public item identity. UID identifies the physical tag, not the Ark character/item.

## Example

```json
{
  "ark": {
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
    "image": "https://raw.githubusercontent.com/NFCSPOOFER/YWAPI/main/images/ark/front/YW-ARK-0338.png",
    "imageBack": "https://raw.githubusercontent.com/NFCSPOOFER/YWAPI/main/images/ark/back/YW-ARK-0338.png",
    "status": "confirmed",
    "scanCount": 2,
    "uniqueTagCount": 2
  }
}
```
