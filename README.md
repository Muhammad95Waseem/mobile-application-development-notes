# Mobile App Development (Android) Notes

Course notes for Mobile Application Development using **Java + XML**. Each topic is a separate file, so you can read them in order or jump to the one you need.

## Contents

| # | Topic | What it covers |
|---|---|---|
| 01 | [Android Architecture](01_architecture.md) | The five layers, their components, example |
| 02 | [Types of Layouts](02_layouts.md) | Linear, Constraint, Frame, Relative, Table, Grid, Coordinator |
| 03 | [LinearLayout](03_linearlayout.md) | Attributes, gravity vs layout_gravity, layout_weight maths |
| 04 | [Responsive Layouts](04_responsive_layouts.md) | ScrollView, landscape layouts, resource qualifiers |
| 05 | [Intents](05_intents.md) | Explicit and implicit intents, extras, code examples |
| 06 | [Intent Filters](06_intent_filters.md) | Declaring filters in the manifest, how matching works |
| 07 | [Activity Lifecycle](07_activity_lifecycle.md) | Callbacks, diagram, common sequences |
| 08 | [Services](08_services.md) | Started, bound and foreground services, lifecycle |
| 09 | [RecyclerView and ListView](09_recyclerview_listview.md) | Adapter, ViewHolder, LayoutManager, comparison |
| 10 | [ImageView](10_imageview.md) | Attributes, scaleType, changing images in code |
| 11 | [Practice Questions](11_practice_questions.md) | MCQs, short answers, scenarios, code, with answers |
| 12 | [Quick Revision](12_quick_revision.md) | One-page summary of everything |

## How to use these notes

- **Learning a topic:** read the file top to bottom. Each one follows the same pattern: definition, how it works, code example, common mistakes, quick summary.
- **Revising:** read the "Quick summary" at the end of each file, then use `12_quick_revision.md`.
- **Testing yourself:** try `11_practice_questions.md` before checking the answers.
- **Code:** examples are in Java and XML. Try them in Android Studio rather than only reading them. The weight examples in `03_linearlayout.md` are good ones to test.

## Notes

- The architecture in `01_architecture.md` follows the five-layer version taught in our course.
- Some details change between Android versions (for example foreground service permissions and `IntentService` being deprecated). Where this matters, the file says so, with a link to the official docs.
- Reference links point to the official [Android Developers](https://developer.android.com) documentation and [GeeksforGeeks](https://www.geeksforgeeks.org/android/android-architecture/).

## Contributing

Found a mistake or want to add something?

1. Fork the repo and create a branch.
2. Edit the relevant `.md` file (keep the same structure: definition, example, common mistakes, quick summary).
3. Open a pull request with a short description of the change.

New topics: use the next number, e.g. `13_fragments.md`, and add a row to the table above.
