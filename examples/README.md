# Compact numerical check

[← Research and full analysis](../README.md)

Run these commands from the repository root.



```sh
python examples/verify_beijing.py
```

Input: `data/beijing-pairs.csv`, 13,943 rows. `svvt` is the street-view vegetation +
terrain proportion; NDVI is summarized in 30, 50 and 100 m road buffers. `review_row`
is a row index, not a location identifier. This table contains no geometry or raw images.

Expected Pearson r: 0.5445584033 / 0.4508014493 / 0.3124452569.
At 100 m, 5,400 roads have discordant median-based categories (HL or LH).
Between 30 and 100 m, 2,810 roads change category. H means at or above the sample
median. These categories express relative positions in this sample.

The script is adapted from the project's existing verification code with only its
default input path changed. See [license notes](../LICENSE_NOTES.md) for the source terms
and OpenStreetMap/USGS attribution. Beijing data and original source code follow
the existing release's CC BY 4.0 terms to the extent of the author's rights.



The reference output is recorded in [expected-results.json](expected-results.json).
