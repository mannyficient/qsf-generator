# Qualtrics QSF Encoding Reference

> **This is the source of truth.** Claude reads this file before generating any QSF. Every JSON shape here was extracted from real Qualtrics exports — not documentation.

## Status

🚧 **This file needs to be populated with real encoding examples.**

To build this out, export a `.qsf` from Qualtrics that contains the question types you use, then document the exact JSON structure for each. See sections below for what to capture.

---

## File-Level Structure

A `.qsf` file is JSON with this top-level shape:

```json
{
  "SurveyEntry": { ... },
  "SurveyElements": [ ... ]
}
```

### SurveyEntry

Contains metadata: `SurveyID`, `SurveyName`, `SurveyOwnerID`, `SurveyBrandID`, etc. Qualtrics overwrites most IDs on import, so placeholder values are fine.

### SurveyElements

An array of element objects. Every survey needs these element types:

| Element | `Element` value | Purpose |
|---|---|---|
| Block List | `BL` | Defines question grouping / pages |
| Survey Flow | `FL` | Controls block ordering and flow logic |
| Project | `PROJ` | Project-level settings |
| Question Count | `QC` | Total question count |
| Response Set | `RS` | Response collection config |
| Scoring | `SCO` | Scoring definitions |
| Survey Options | `SO` | Global survey settings |
| Statistics | `STAT` | Statistics config |
| Survey Question | `SQ` | One per question |
| Page Layout | `PL` | Page layout settings |

---

## Question Type Encodings

### MC / SAVR (Single-Select)

<!-- TODO: Paste exact JSON shape from a real export -->

```json
{
  "SurveyID": "SV_PLACEHOLDER",
  "Element": "SQ",
  "PrimaryAttribute": "QID1",
  "SecondaryAttribute": "What is your role?",
  "TertiaryAttribute": null,
  "Payload": {
    "QuestionText": "What is your role?",
    "QuestionDescription": "What is your role?...",
    "QuestionType": "MC",
    "Selector": "SAVR",
    "SubSelector": "TX",
    "Choices": {
      "1": { "Display": "Designer" },
      "2": { "Display": "Developer" },
      "3": { "Display": "PM" }
    },
    "ChoiceOrder": ["1", "2", "3"],
    "Configuration": {
      "QuestionDescriptionOption": "UseText"
    },
    "DataVisibility": {
      "Private": false,
      "Hidden": false
    },
    "Validation": {
      "Settings": { "Type": "None" }
    },
    "Language": [],
    "QuestionID": "QID1",
    "DataExportTag": "Q1"
  }
}
```

### MC / MAVR (Multi-Select)

<!-- TODO: Paste exact JSON shape from a real export -->

### MC / NPS (Net Promoter Score)

<!-- TODO: Document the array-based Choices and ColumnLabels encoding -->

### TE / SL (Text Entry — Single Line)

<!-- TODO -->

### TE / ML (Text Entry — Multi Line)

<!-- TODO -->

### Matrix / Likert

<!-- TODO: Document Choices (rows), Answers (columns), Autoscale config -->

### RO / DND (Rank Order — Drag and Drop)

<!-- TODO -->

### DB / TB (Descriptive Text Block)

<!-- TODO -->

### MC / DL (Dropdown)

<!-- TODO -->

### Slider / HSLIDER

<!-- TODO -->

---

## Block Encoding

<!-- TODO: Document BlockElements structure, Page Break encoding -->

---

## Survey Flow Encoding

<!-- TODO: Document FL element shape, EmbeddedData, Branch logic -->

---

## Validation / Force Response

<!-- TODO: Document force response ON vs OFF encoding -->

---

## Known Gotchas

1. `ChoiceOrder` values **must be strings** — `["1","2","3"]` not `[1,2,3]`
2. `QuestionDescription` must be truncated to ~100 chars with `...` suffix
3. NPS `Choices` is an **array**, all other types use a keyed dict
4. NPS requires `ColumnLabels` with `IsLabelDefault: true`
5. Matrix needs `Autoscale` and several layout fields in `Configuration`
6. TE/SL needs `SearchSource: {"AllowFreeResponse": "false"}`
7. Omit `ForceResponseType` when validation is OFF — use `{"Settings": {"Type": "None"}}`
8. `DefaultChoices` and `GradingData` are required on RO, NPS, Matrix, DB
9. Qualtrics reassigns `SurveyID`, `RS`, `PreviewID` on import — placeholders are fine
10. Page breaks in blocks: `{"Type": "Page Break"}` in `BlockElements`
