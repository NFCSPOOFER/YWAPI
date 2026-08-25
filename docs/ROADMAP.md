# Roadmap

YWAPI starts with Yo-kai Arks because that is the first scanned and decoded dataset.

## Current

- Publish static Yo-kai Ark API.
- Include front and back PNG assets directly in the repository.
- Keep app-friendly lookup endpoints for decoded NFC identity.
- Track scan status as `confirmed`, `single_scan`, `missing`, or `conflict`.

## Next

- Resolve remaining conflicts as more community scans arrive.
- Add missing backs and improved preservation images.
- Add richer catalog metadata when verified.
- Publish stable versioned exports.

## Later Sections

The API structure is ready to expand into:

- Dream Medals
- Treasure Medals / T Medals
- Yo-seiken
- Yo-kai Y Medals
- Genju Disks

Each section should get its own folder under `/api`, its own images folder, and the same style of index, lookup, status, and CSV exports.
