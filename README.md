# Burmese Font Detector (Zawgyi vs Unicode)

A Google Colab notebook that detects whether Burmese (Myanmar) text is written in **Zawgyi** or **Unicode**. It works on a single piece of text or on a whole column of an Excel file.

## Why this matters

Zawgyi and Unicode use the same character range but encode Burmese differently. The same text can look correct in one and garbled in the other. When a dataset mixes both, search, sorting and text processing break. Detecting the encoding is the first step to cleaning that data.

## What it does

1. **Single text check:** paste Burmese text and get `Zawgyi`, `Unicode`, or `Not Burmese text`.
2. **Excel check:** upload an `.xlsx` file, pick the column with Burmese text, and the notebook adds a new `Font_Type` column with the result for every row. The result is downloaded as `result.xlsx`.

## How it works

The notebook uses the `ZawgyiDetector` from Google's [Myanmar Tools](https://github.com/google/myanmar-tools) library. It returns the probability (0 to 1) that a text is Zawgyi:

| Score | Result |
|---|---|
| No Burmese characters | Not Burmese |
| 0.5 or higher | Zawgyi |
| Below 0.5 | Unicode |

Blank or non-text cells are left empty.

## How to use

1. Open `burmese_font_detector.ipynb` in Google Colab.
2. Run the install cell:
   ```python
   !pip install git+https://github.com/google/myanmar-tools.git#subdirectory=clients/python openpyxl pandas -q
   ```
   (`pip install myanmartools` also works.)
3. For a single text, run the input cell and paste your text.
4. For an Excel file, run the upload cell, choose your file, and set `col` to the column that holds the Burmese text. If your column has no header, pandas names it `Unnamed: 0`.
5. The result file `result.xlsx` is downloaded with the new `Font_Type` column.

## Sample data

`sample_data/burmese_test_data.xlsx` contains 16 rows of mixed Zawgyi, Unicode and non-Burmese text with an answer key column, so you can check that the notebook works. On this file the detector labels all 16 rows correctly.

Zawgyi text will look garbled in Excel and in the Colab preview. This is expected, because the preview renders text as Unicode. The detection is not affected.

## Limitations

- Very short text, such as a single word, can score close to 0.5 and be misdetected. Longer text is more reliable.
- The detector only tells Zawgyi from Unicode. It cannot identify other legacy Burmese fonts.
- Only one column is checked at a time.

## Possible improvements

- Convert Zawgyi rows to Unicode in a new column
- Add an "Uncertain" label for scores near the middle
- Support CSV files and several columns
- Evaluate accuracy on a larger labeled dataset
- Build a simple web app for pasting text or uploading files

## Credits

Detection is powered by [Google Myanmar Tools](https://github.com/google/myanmar-tools), released under the Apache 2.0 license.
