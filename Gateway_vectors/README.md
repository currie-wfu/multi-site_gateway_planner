# Adding Vectors to the Gateway Website

## Overview

The website loads its vector library from two things:
1. The GenBank file (`.gb`) placed in this folder (`Gateway_vectors/`)
2. An entry in `vectors.json` that tells the website the file exists and how to label it

Both steps are required. A `.gb` file with no JSON entry won't appear, and a JSON entry pointing to a missing file will cause an error when a user tries to load it.

---

## Step 1 — Add the GenBank file

Copy or save your `.gb` file into this folder (`Gateway_vectors/`). The filename can contain spaces, parentheses, or hyphens — the website handles these correctly. Keep the filename exactly as you intend to reference it in the JSON.

---

## Step 2 — Edit `vectors.json`

Open `vectors.json` and add a new entry inside the `"vectors": [ ... ]` array. Each entry has four fields:

```json
{
  "file": "JDW_1234_pME-MyInsert.gb",
  "name": "pME-MyInsert",
  "type": "Middle Entry",
  "description": "Middle Entry clone with my insert of interest"
}
```

| Field | What it does |
|---|---|
| `file` | Exact filename of the `.gb` file in this folder — must match character for character including capitalization |
| `name` | Short display name shown in the dropdown menus on the website |
| `type` | Category label — controls which dropdown the vector appears in (see below) |
| `description` | Brief description shown in the vector info panel |

### Valid `type` values

| Type string | Dropdown it appears in |
|---|---|
| `"5' Entry"` | 5′ Element dropdown |
| `"Middle Entry"` | Middle Element dropdown |
| `"3' Entry"` | 3′ Element dropdown |
| `"Destination"` | Destination vector dropdown |

The type string must be written exactly as shown above (including the apostrophe and space).

### Where to insert the new entry

Entries within each type are displayed in the order they appear in the JSON. Insert your new entry near other vectors of the same type to keep the dropdown organized. For example, add a new Middle Entry clone near the other `"Middle Entry"` entries.

---

## Example — adding a new 3′ Entry clone

1. Copy `JDW_1400_p3E-mNeonGreen-SV40pA.gb` into this folder.
2. Open `vectors.json` and find the `"3' Entry"` block near the bottom.
3. Add the new entry after the last `"3' Entry"` entry:

```json
    {
      "file": "JDW_1400_p3E-mNeonGreen-SV40pA.gb",
      "name": "p3E-mNeonGreen-SV40pA",
      "type": "3' Entry",
      "description": "3' Entry clone with mNeonGreen and SV40 polyA signal"
    }
```

4. Make sure every entry except the last one in the array ends with a comma. Standard JSON — no trailing comma after the final entry before the closing `]`.

5. Reload the website. The new vector should appear in the 3′ Element dropdown.

---

## Changing the default Entry clone template

The website uses specific Entry clones as templates when generating GenBank files for user-defined raw sequences (the "Generate Entry vector GenBank" feature). These templates are hardcoded in `index.html` and are separate from the dropdown list in `vectors.json`.

To change a default template, search `index.html` for `loadPDONRBackbones` and update the filename for the relevant vector type:

```javascript
const templateFiles = {
    'p4p1r': 'JDW_404_p5E-UASE1b.gb',           // 5' Entry template
    '221':   'JDW_455_pME-MCS (WPD).gb',          // Middle Entry template
    'p2rp3': 'JDW_967_p3E-H2A_mCherry_SV40pA.gb'  // 3' Entry template
};
```

The replacement file must:
- Already exist in this `Gateway_vectors/` folder
- Also be listed in `vectors.json` (so it can be loaded by the website)
- Have clearly labeled `attL1`/`attL2` (Middle), `attL4`/`attR1` (5′), or `attR2`/`attL3` (3′) features in its GenBank annotation

---

## Troubleshooting

**Vector doesn't appear in the dropdown**
- Check that the `type` string in `vectors.json` matches exactly (including capitalization and the apostrophe in `5' Entry` / `3' Entry`).
- Confirm the JSON is valid — a missing comma or extra bracket will prevent all vectors from loading. Paste the file into [jsonlint.com](https://jsonlint.com) to check.

**"Could not load vector" error when selecting a vector**
- The `"file"` value in `vectors.json` doesn't match the actual filename. Check for typos, extra spaces, or capitalization differences.

**Entry clone GenBank output has incorrect att sites**
- The template file being used may have an inaccurate att site annotation. Switch to a different template (see "Changing the default Entry clone template" above).
