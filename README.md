### 🔧 **Final Execution: Parity Check Between BHK and Carpet Data (Before & After Fixes)**

This cell performs a parity validation between two different representations of unit counts in the dataset: the BHK summary (`bhks`) and the Carpet-wise totals (`Carpet_wise_total_sold`). The checks are performed twice — before and after cleaning/alignment fixes — and the results are saved for manual review.

**What It Does**
- **Extracts totals**: computes total units and sold units from the `bhks` structure.
- **Extracts carpet totals**: computes total units and sold units from `Carpet_wise_total_sold` (raw before processing).
- **Preserves originals**: saves the original carpet and BHK-CA data before any modification for auditing.
- **Removes duplicate index**: drops the 0th index from both `Carpet_wise_total_sold` and `BHK_wise_CA` when lists contain a redundant 0th element.
- **Aligns lists**: pads and aligns `{}` entries so the carpet data and BHK CA lists match the `bhks` length, preventing mismatches due to missing entries.
- **Recalculates**: recomputes carpet totals after alignment.
- **Adds parity flags**:
  - `parity_before`: True when totals and sold counts match between `bhks` and carpet data before fixes.
  - `parity_after`: True when totals and sold counts match after alignment/fixes.
- **Outputs**: writes a workbook/CSV containing the computed columns to allow manual cross-verification.

**Key Columns Produced**
- `bhk_total`, `bhk_sold`, `bhk_count` — totals from the `bhks` structure.
- `carpet_total_before`, `carpet_sold_before`, `carpet_count_before` — totals calculated from `Carpet_wise_total_sold` before fixes.
- `carpet_total_after`, `carpet_sold_after`, `carpet_count_after` — totals calculated after alignment and fixes.
- `parity_before`, `parity_after` — boolean indicators of parity match.

**Files / Locations**
- Notebook: the parity logic is in the notebook `perity checking after o1g.ipynb` in this folder.
- Example exported result (from the notebook code): `Demo_bhk_wise_CA_parity.xlsx` (path may vary depending on your environment). Look for the parity columns above.

**Prerequisites**
- Python 3.8+ (or the environment used by the notebook).
- Required Python packages (typical): `pandas`, `ast` (stdlib). The notebook may use additional libs elsewhere (e.g., `selenium`, `bs4`) but parity-check cells rely primarily on `pandas` and `ast`.

**How to Run (Notebook)**
- Open `perity checking after o1g.ipynb` in your Jupyter/VS Code notebook environment.
- Locate the cell with the heading shown at the top of this README (this is the parity-check cell).
- Run the cell. It will:
  - Read the DataFrame you have loaded earlier in the notebook.
  - Produce the parity calculation columns in-memory and save an output workbook (if save path is enabled in the cell).

If you want to run only the parity-check logic as a standalone script, extract the parity-check cell code into a Python file and ensure:
- `df` (the DataFrame) is loaded first and contains the columns used by the cell (e.g., `BHKs_Updated`, `Carpet_wise_total_sold_converted_avg_converted`, `BHK_wise_CA_Updated`).
- Then run the parity functions (parsing, alignment, totals extraction, and parity flags).

**Quick Troubleshooting**
- Parsing errors: many fields are stored as stringified Python literals. If you receive `ValueError` or `SyntaxError` from `ast.literal_eval`, inspect the offending row and normalize the string (e.g., ensure proper bracket/quote usage).
- Missing columns: confirm your DataFrame column names match those expected by the notebook (spelling and casing). Use `df.columns` to inspect.
- Unexpected data shapes: some rows contain `[]`, `{}`, or malformed dicts — the cell pads lists and handles these cases, but review rows that still fail parity to understand manual edge-cases.

**Next Steps / Suggestions**
- If many rows remain `False` for `parity_after`, export those rows to a CSV for manual inspection and fix rules (e.g., unit mapping issues, mixed sqft/sqm values).
- Consider adding a small validation cell earlier that normalizes stringified columns into Python objects (lists/dicts) to ensure downstream cells are simpler and safer.

**Contact**
- If you want this README adjusted (for example, to change wording to "parity" instead of "perity"), I can update the filename and this document.

---
Generated to document the parity-check starting at the specified cell in `perity checking after o1g.ipynb`.