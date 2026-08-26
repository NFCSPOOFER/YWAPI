# MyTags Structure Notes

This document records the current community-facing structure notes for Yo-kai Watch NFC items in YWAPI.

It follows the same idea as the community Puni Puni / Medaland QR workbook: separate item families, keep source/distribution context visible, and preserve technical identifiers without mixing them into display names.

## Terminology

| Term | Meaning |
| --- | --- |
| `ItemID` | Preferred term for the decoded numeric NFC identity used by the game after decrypting/reading a tag payload. |
| `displayCode` | Seven-character Ark display code, such as `0103MBW`. |
| `arkKey` | Three-character key inside the display code, such as `MBW`. |
| `numericId` | Early YWAPI compatibility alias for `ItemID`. |
| CRC-32B ID | Level-5 engine ID/hash value. CRC-32B / ISO-HDLC is the default CRC-32 variant used for many config identifiers. |
| UID | Physical NTAG UID. This identifies the physical tag, not the Yo-kai Watch item identity. |

## Important Distinction

`nfc_lottery_config.cfg.bin` is the NFC reward-routing config.

`item_config_0.13.66.cfg.bin` is item config.

The mapper uses both:

1. The NFC config maps scanned NFC `ItemID` values to reward table references.
2. Reward table entries contain CRC-32B-style item hash/ID values.
3. Item config resolves those reward item hashes into internal item identifiers.
4. `item_text.cfg.bin` resolves item names/descriptions for those item identifiers.

So if an output shows reward item names, that part came from item config/text config. If it shows which NFC identity points to which reward table, that part came from NFC config.

## Current Ark / Keystone Fields

| Field | Example | Notes |
| --- | --- | --- |
| `id` | `YW-ARK-0338` | Public YWAPI catalog ID. |
| `legacyId` | `YKW-ARK-0338` | Original internal project ID retained for compatibility. |
| `name` | `Wildfire Blaze` | Public item name. |
| `type` | `Ark` | Public family/type. |
| `series` | `Sacred Armory` | Specific product/set grouping when known. |
| `displayCode` | `0103MBW` | Decoded Ark display code. |
| `arkKey` | `MBW` | Key portion of the display code. |
| `numericId` | `28940` | Compatibility alias for `ItemID`. |
| `numericIds` | `[28940]` | All known ItemIDs for the catalog row. |
| `image` | front PNG URL | Transparent front image. |
| `imageBack` | back PNG URL | Transparent back image when available. |

## Product Families

The source QR workbook organizes Yo-kai Watch NFC/QR collectibles by family tabs. YWAPI should follow that pattern as more data is added:

| Family | Current YWAPI status |
| --- | --- |
| Yo-kai Arks / Keystones | Active |
| Yo-seiken / Sacred Armory | Included in the catalog with `type: "Yo-seiken"`; endpoints remain under `/api/ark/` for compatibility. |
| Dream Medals | Planned |
| Treasure Medals / T Medals | Planned |
| Yo-kai Y Medals | Planned |
| Genju Disks | Planned |
| Other Medals | Planned / research only |

## Game Reward Fields

| Field | Meaning |
| --- | --- |
| `nfcItemId` | Preferred field for the decoded NFC ItemID. |
| `nfcNumericId` | Compatibility alias for `nfcItemId`. |
| `rewardTables[].tableHash` | CRC-32B-style table ID from NFC config. |
| `rewardTables[].rewards[].itemHash` | CRC-32B / ISO-HDLC reward item ID/hash from the reward table. |
| `rewardTables[].rewards[].hashAlgorithm` | Hash algorithm note for `itemHash`. |
| `rewardTables[].rewards[].itemId` | Internal item identifier resolved from item config, such as `iky010020`. |
| `rewardTables[].rewards[].itemKey` | Compatibility alias for `itemId`. |
| `rewardTables[].rewards[].nameJa` | Japanese reward item name from item text. |
| `rewardTables[].rewards[].nameEn` | Reviewed English reward item name. |
| `rewardTables[].rewards[].descriptionJa` | Japanese reward item description from item text. |
| `rewardTables[].rewards[].descriptionEn` | Machine-draft English reward item description. |
| `rawRewardTypeOrSlot` | Raw context-dependent table value preserved from game config. |
| `quantityOrWeight` | Raw config value preserved from game config. |

## Known Split: YW-ARK-0340

`YW-ARK-0340` was originally cataloged as one combined row for Enma Blade and Jaou Ken. Community Unknown/Other scans confirmed it represents two distinct NFC identities:

| Item | Display code | Ark key | ItemID | Evidence |
| --- | --- | --- | ---: | --- |
| Enma Blade | `0106MBZ` | `MBZ` | `28943` | Unknown/Other submission and live scan grouping. |
| Jaou Ken / Ananta Blade | `0107MC0` | `MC0` | `28944` | Unknown/Other submission and live scan grouping. |

The public reward export marks these as `identified_split` until the catalog rows themselves are split cleanly.

## Preservation Notes

- Keep UID data out of public identity matching. UID is useful for scan uniqueness, not item identity.
- Keep compatibility aliases until early app/API users can migrate.
- Mark game-config-derived data separately from community scan catalog data.
- Mark machine translations separately from reviewed translations.
