# IGL CNG Outlet Data Scraper

Selenium-based Python automation that visits configured IGL outlet pages, extracts data from the first two HTML tables on each page, consolidates the results, and creates a multi-sheet Excel workbook.

## How it works

1. Opens the IGL website in a visible Chrome browser.
2. Visits each entry in the script's configured AREAS list.
3. Waits for the rendered page and its tables, then parses the HTML with BeautifulSoup.
4. Processes up to the first two tables on each page. Rows are classified as CNG Station when the headers contain Phone Number, as OMC Outlet when they contain OMC, or otherwise as a numbered table.
5. Adds area/table/source metadata, combines the records, removes blank station names and duplicate rows, and creates summary/log sheets.

## Output workbook

The script writes IGL_CNG_Stations_All_Areas.xlsx with three sheets:

- All_CNG_Stations — combined records with available source columns plus Area, Table_Type, and Source_URL
- Scrape_Log — per-area success/failure status, record counts, and error text
- Summary — total areas, successful/empty areas, records, and duplicates removed

The workbook is formatted with a frozen header row, filters, and adjusted column widths.

## Diagnostics

- IGL_Debug_HTML/ — rendered page HTML saved for each configured area
- IGL_Debug_Screenshots/ — screenshots saved when an area fails

These diagnostics help inspect changes in the source page or selector behavior.

## Requirements

- Python 3.10 or a compatible version
- Packages in requirements.txt
- Google Chrome installed and compatible with Selenium WebDriver

## Setup and run

    python -m venv .venv
    .venv/Scripts/Activate.ps1
    pip install -r requirements.txt
    python Indraprastha_Gas_Limited_IGL.py

## Configuration

Review the following values in Indraprastha_Gas_Limited_IGL.py:

- BASE_URL — source website
- AREAS — the fixed list of outlet pages the script visits
- OUTPUT_FILE — workbook name
- WAIT_TIME and PAGE_DELAY — page wait and pacing settings

The script uses ordinary Chrome (visible by default) and saves output in the current working directory.

## Notes

- The scraper processes only the first two tables found on each configured area page.
- If no data is found, inspect the diagnostics folders; the site's structure may have changed or an area page may have failed to load.
- Respect source-site terms and applicable policies when collecting or redistributing data.
