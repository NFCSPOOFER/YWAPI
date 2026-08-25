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
| By ItemID / numeric ID | `GET /api/ark/by-numeric-id/28940.json` |
| By series | `GET /api/ark/by-series/sacred-armory.json` |
| By status | `GET /api/ark/by-status/confirmed.json` |

## Game Reward Endpoints

These endpoints expose decoded Yo-kai Watch 4++ NFC reward config data. In Level-5 terminology, the decoded numeric NFC identity is treated as an `ItemID`; older YWAPI fields using `numericId` are retained as compatibility aliases.

| Purpose | Request |
| --- | --- |
| All decoded reward entries | `GET /api/game-rewards/index.json` |
| All decoded reward entries alias | `GET /api/game-rewards/all.json` |
| Catalog-matched reward entries | `GET /api/game-rewards/matched.json` |
| Unmatched reward entries | `GET /api/game-rewards/unmatched.json` |
| By NFC ItemID / numeric ID | `GET /api/game-rewards/by-numeric-id/28707.json` |
| By catalog ID | `GET /api/game-rewards/by-catalog-id/YW-ARK-0001.json` |
| Reward summary | `GET /api/game-rewards/summary.json` |

Reward entries include `confidence: "decoded_config"` and `validationStatus: "not_gameplay_verified"` until the decoded tables are validated against real game behavior.

English reward item names are currently reviewed. English reward item descriptions are included as machine-draft translations and should be treated as helpful provisional text, not final localization.

Catalog ID reward lookups return an array because a small number of catalog entries currently have multiple decoded reward mappings while conflicts/special items are being resolved.

## Index Endpoints

| Purpose | Request |
| --- | --- |
| API summary | `GET /api/index.json` |
| Supported item types | `GET /api/type/index.json` |
| Known Ark series | `GET /api/series/index.json` |
| Status values and counts | `GET /api/status/index.json` |
| Last generated timestamp | `GET /api/lastupdated/index.json` |

## Game Reward Field Reference

| Name | Type | Description |
| --- | --- | --- |
| `nfcItemId` | number | Preferred field name for the decoded NFC ItemID used by the game config. |
| `nfcNumericId` | number | Compatibility alias for `nfcItemId`. |
| `catalogIds` | string[] | Matching YWAPI catalog IDs, when known. |
| `catalogNames` | string[] | Matching YWAPI catalog names, when known. |
| `familyGuess` | string | Research classification such as `yo-kai-ark`. |
| `matchStatus` | string | `catalog_match`, `unmatched_near_ark_range`, or `unmatched`. |
| `writerReady` | boolean | Whether this identity exists in the confirmed writer library. |
| `confidence` | string | Current confidence level for decoded config data. |
| `validationStatus` | string | Gameplay validation state. |
| `catalogSplitStatus` | string | Optional marker for known combined catalog entries that have been split or need variant assignment. |
| `knownCombinedCatalogId` | string | Optional original combined catalog ID, when an entry is being split. |
| `catalogVariantName` | string | Optional identified variant name for a split catalog row. |
| `catalogVariantDisplayCode` | string | Optional display code for the identified split variant. |
| `catalogVariantArkKey` | string | Optional Ark key for the identified split variant. |
| `catalogVariantVisual` | string | Optional short visual description for the identified split variant. |
| `catalogSplitEvidence` | string | Optional evidence note for the split mapping. |
| `rewardTables` | array | Reward table slots referenced by this NFC identity. |
| `rewardTables[].rewards[].itemKey` | string or null | Internal item key guess, such as `iky010020`. |
| `rewardTables[].rewards[].itemHash` | string or null | CRC-32B/ISO-HDLC ID value from the game config. |
| `rewardTables[].rewards[].nameJa` | string or null | Japanese item name decoded from game text. |
| `rewardTables[].rewards[].nameEn` | string or null | English item name when available. |
| `rewardTables[].rewards[].nameEnStatus` | string | Translation status. Current reviewed names use `reviewed`. |
| `rewardTables[].rewards[].descriptionJa` | string or null | Japanese item description decoded from game text. |
| `rewardTables[].rewards[].descriptionEn` | string or null | English item description when available. |
| `rewardTables[].rewards[].descriptionEnStatus` | string | Translation status. Current machine-generated descriptions use `machine_draft`. |
| `rewardTables[].rewards[].descriptionEnNotes` | string or null | Translation notes/source, when available. |
| `rewardTables[].rewards[].quantityOrWeight` | number or null | Raw quantity/weight value from the reward table. |
| `rewardTables[].rewards[].rewardValueSemantics` | string | Current interpretation of `quantityOrWeight`; values remain unverified until gameplay-tested. |
| `rewardTables[].rewards[].resolutionStatus` | string | How the reward hash was resolved, such as `parsed_item_config_match`. |

## Game Reward Example

```json
{
  "gameReward": {
    "nfcItemId": 28638,
    "nfcNumericId": 28638,
    "catalogIds": ["YW-ARK-0315"],
    "catalogNames": ["Jibanyan (SS)"],
    "familyGuess": "yo-kai-ark",
    "matchStatus": "catalog_match",
    "confidence": "decoded_config",
    "validationStatus": "not_gameplay_verified",
    "translationStatus": "mixed_reviewed_and_machine_draft",
    "rewardTables": [
      {
        "slot": 0,
        "tableHash": "72165E6E",
        "resolvedRewardCount": 103,
        "rewards": [
          {
            "itemHash": "D34B5E2F",
            "itemKey": "iky010010",
            "nameJa": "うめおにぎり",
            "nameEn": "Plum Rice Ball",
            "nameEnStatus": "reviewed",
            "descriptionJa": "すっぱい梅干しが入っている　おにぎり。\nおにぎりといえばコレ！　という人も多い。",
            "descriptionEn": "Rice balls with sour plums. Many people say that this is the best rice ball.",
            "descriptionEnStatus": "machine_draft",
            "quantityOrWeight": 18,
            "rewardValueSemantics": "raw_weight_or_quantity_unconfirmed"
          }
        ]
      }
    ]
  }
}
```

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
| `numericId` | number or null | Compatibility field for the primary decoded NFC ItemID. |
| `numericIds` | number[] | Compatibility field for all known decoded NFC ItemIDs. |
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
