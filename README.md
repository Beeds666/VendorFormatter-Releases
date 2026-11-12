# Vendor File Formatter

Formats vendor product lists, invoices and open orders into workbooks ready for AEPOS (AdvEntPOS), fills in item
details from your AEPOS database, and exports the CSV files AEPOS imports. Part of **Backroom Suite**; independent
software, not made or supported by AdvEntPOS.

This repository only holds the installers. Most stores install it through the Backroom Suite hub
([BackroomSuite-Releases](https://github.com/Beeds666/BackroomSuite-Releases)), which also keeps it updated.

## Stable and test versions

New versions are published first as **test versions** (marked *Pre-release* on the Releases page), then marked as
released for everyone once they've been used at a test store.

| | Stable (recommended) | Test |
|---|---|---|
| On the [Releases](../../releases) page | **Latest** (not marked *Pre-release*) | Marked *Pre-release* |
| Who gets it as an update | Every PC | Only PCs on the test channel |
| How a PC opts in | Default | Backroom Suite hub → tick **Get test versions first** (applies to the hub and every suite app on that PC). Untick it to go back to stable; a newer test version stays installed until the next stable one. |

At the time of writing (November 2026) the stable version is **2.6.0** and **2.8.0** is a test version. The
Releases page is always the current answer: the release marked **Latest** is the stable one.

## Install

1. Download `VendorFormatterSetup-x.y.z.exe` from the release marked **Latest** (or install from the Backroom Suite hub).
   Each release also has `VendorFormatterSetup-x.y.z.exe.sha256`; the in-app updater checks downloads against it
   (by hand: `Get-FileHash .\VendorFormatterSetup-x.y.z.exe` in PowerShell must show the same value).
2. Run it (Windows asks for administrator permission). The installers aren't code-signed yet, so SmartScreen may
   warn: **More info → Run anyway**.
3. On first launch **Database settings** opens: enter the AEPOS SQL Server and login (or pick a store already set up
   in the Backroom Suite hub), click **Load list** to pick the database, keep the view `dbo.VF_ItemLookup`
   (**Create view…** if the store doesn't have it yet), **Save**, then **Test database**.

## Requirements

- Windows 10 or 11, 64-bit. Nothing else to install for the app itself.
- Network access to the store's **AEPOS SQL Server** for item look-ups, comparing exports with AEPOS and the reports.
  PostgreSQL (Sypram cloud) stores aren't supported by this app.
- A spreadsheet program, depending on what you use:

| Feature | Needs |
|---|---|
| Format a vendor file (all templates) | Microsoft Excel **or** any other spreadsheet program (LibreOffice Calc, WPS Spreadsheets, GridCraft…): without Excel the built-in engine does the work and the result opens in the program you pick |
| Run a template as its original VBA macro | Microsoft Excel, with "Trust access to the VBA project object model" turned on (the app asks on the first run) |
| Export for import (CSV), AEPOS data check, reports | No spreadsheet program |
| Scanned invoices (PDF or photo: L.J. Zucca, Lipari Foods) | No spreadsheet program (text recognition is built in) |
| Checks to the bookkeeping workbook | Nothing while the workbook is closed. If it's open, it must be open in Microsoft Excel (the check is added there; save it in Excel) |

## Supported vendor files

| Vendor file | Template |
|---|---|
| C&S product list (Data Download, .xlsx or .csv) | C&S Data Download |
| C&S invoice export (`Invoice_<number>_<date>.xlsx`, with its PDF) | C&S Invoice |
| C&S open orders (`Open_Orders_<date>.xlsx`) | C&S Open Orders |
| L.J. Zucca invoice (scan or PDF) | L.J. Zucca Invoice (scan) |
| Lipari Foods invoice (scan or phone photo) | Lipari Foods Invoice (scan) |

Other vendors can be added as templates (a VBA `.bas` plus a `.json`).

## Basic use

1. Drag the vendor file onto the window (or choose it). The template is picked from the file.
2. Answer the prompts (margin, vendor…) and click **Run**. The app copies the file (the original is never changed),
   formats it, looks every UPC up in AEPOS and opens the result. Items not found in AEPOS are highlighted.
3. Edit the workbook as needed and save it.
4. **Export for import…**: pick the saved workbook. Choose one CSV with every row, or the 4 CSVs the app makes by
   comparing with AEPOS, next to the workbook:
   `- New` (items not in AEPOS), `- Updates` (changed items), `- Undo` (AEPOS's values before the import) and
   `- Changes` (every field that changes). Import those into AEPOS. Files from an earlier export of the same
   workbook are moved to an **Old exports** folder, and `<workbook> - Export info.txt` lists exactly which files the
   current export made.
5. Invoices: the shipped lines also go to Backroom Inventory's **Receive → Import invoice** (if installed), and the
   app offers to record the check in the bookkeeping workbook.

Settings, templates and logs: `C:\ProgramData\VendorFormatter`. Database connection: `%LocalAppData%\VendorFormatter`
and the suite's shared `%LocalAppData%\BackroomSuite\stores.json` (passwords encrypted for the Windows user).

## Updates

The app checks for a newer version when it opens (and with **Check for updates**) and offers **Update now**. The
download must match the release's checksum and is checked again just before it runs. Settings, templates,
connections and history are kept.

## Release notes

Each release lists what changed, known issues, and anything to do when upgrading.
