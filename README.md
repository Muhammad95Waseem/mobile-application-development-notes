# Android (MAD) Notes

Theory notes for Mobile Application Development (Android, Java + XML). Each topic is in its own file, so you can read them one by one in order.

## Contents

| # | File | Topic |
|---|---|---|
| 01 | [01_architecture.md](01_architecture.md) | Android architecture (5 layers) |
| 02 | [02_layout_types.md](02_layout_types.md) | Layout types |
| 03 | [03_linear_layout.md](03_linear_layout.md) | LinearLayout |
| 04 | [04_responsiveness_of_linear_layout.md](04_responsiveness_of_linear_layout.md) | Responsiveness of LinearLayout |
| 05 | [05_image_view.md](05_image_view.md) | ImageView |
| 06 | [06_intent.md](06_intent.md) | Intent |
| 07 | [07_types_of_intent.md](07_types_of_intent.md) | Types of Intent |
| 08 | [08_intent_filter.md](08_intent_filter.md) | Intent Filter |
| 09 | [09_activity.md](09_activity.md) | Activity |
| 10 | [10_activity_lifecycle.md](10_activity_lifecycle.md) | Activity Lifecycle |
| 11 | [11_services.md](11_services.md) | Services |
| 12 | [12_types_of_services.md](12_types_of_services.md) | Types of Services |
| 13 | [13_service_lifecycle.md](13_service_lifecycle.md) | Service Lifecycle |
| 14 | [14_recycler_view.md](14_recycler_view.md) | RecyclerView |
| 15 | [15_recyclerview_vs_listview.md](15_recyclerview_vs_listview.md) | RecyclerView vs ListView |

## How to Use
- Open any `.md` file on GitHub (or in VS Code with Markdown preview) to read it formatted.
- Files are numbered in a sensible learning order: UI first (layouts, views), then components (intents, activities, services), then lists.
- Diagrams are written in plain text so they render everywhere; view them in a monospace font.

## Pushing to GitHub
```bash
git init
git add .
git commit -m "Add Android notes"
git branch -M main
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```
