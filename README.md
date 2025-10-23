# Bitwise Operator Empirical Analysis Replication Package (Quorum)

This repository contains a Quorum Studio project that replicates an empirical methods analysis of bitwise operators. It includes descriptive statistics, one-way ANOVA, Tukey’s HSD multiple comparisons, simple regression, and visualizations (scatter plot and box plots) generated from the provided CSV datasets.

## What’s inside

```
Build/
Data/
  accuracy.csv
  fulldata.csv
  response.csv
  transposedata.csv
  Collected Data/               # Raw data drops (as collected)
  individualopdata/             # Per-operator CSVs (wide + transposed)
Images/
  scatterplot.html
  Boxplot/
    andboxplot.html
    lshiftboxplot.html
    notboxplot.html
    orboxplot.html
    rshiftboxplot.html
    xorboxplot.html
Project/
  Project.qp                    # Quorum project file (main set to regression)
SourceCode/
  Boxplot.quorum                # Generates box plots (HTML exported)
  DescriptiveAnalysis.quorum    # Prints means/SD/variance per column
  oneWayANOVA.quorum            # One-way ANOVA for each operator group
  regression.quorum             # Linear regression on filtered data (Main)
  scatterplot.quorum            # Scatter plot + linear trend (HTML exported)
  Tukey's HSD Multiple Comparison Test.quorum  # Tukey HSD pairwise tests
```

Notes:
- The project’s main entry is `SourceCode/regression.quorum` (configured in `Project/Project.qp`).
- Most scripts print statistical summaries to the console; plotting scripts also export HTML charts under `Images/`.

## Prerequisites

- Quorum Studio 6.0.0 or newer (as indicated by `Project.qp`).
- Windows, macOS, or Linux. The paths in source files primarily use Windows separators (\\); on non-Windows systems, ensure the working directory is the project root so relative paths resolve.

If you don’t have Quorum Studio yet, download and install it from the Quorum Language website. Then open the project as described below.

## Getting started (Windows/Quorum Studio)

1. Clone this repository (replace with your GitHub URL):

```powershell
git clone https://github.com/shubham4564/bitwise_op_empirical_analysis.git
cd bitwise_op_empirical_analysis
```

2. Open Quorum Studio and choose File > Open Project, then select `Project/Project.qp`.
3. Press Run to execute the default main (`regression.quorum`).

If you want to run a specific analysis script, open it from `SourceCode/` and run it directly.

## Data inputs

Primary CSV files used by the analyses:
- `Data/response.csv` — response times for multiple operator spellings.
- `Data/transposedata.csv` — tidy/transposed dataset with columns such as `operators`, `response time`, and `accuracy`.
- `Data/fulldata.csv` — wide format columns named `res_and1..5`, `res_or1..3`, `res_xor1..4`, `res_not1..4`, `res_lshift1..4`, `res_rshift1..4`.
- `Data/individualopdata/*.csv` — per-operator slices (wide) and `*transpose.csv` for long-form.

Keep the working directory at the project root so relative paths work. Some files in `SourceCode/` load CSVs via `Data/...` while others use bare filenames like `fulldata.csv`; both resolve correctly when the project runs from its root.

## How to run each analysis

- Regression (`regression.quorum`)
  - Loads `Data/transposedata.csv`, filters `accuracy = 1`, treats `operators` as a factor, and regresses on `response time`.
  - Output: printed model summary in the Quorum console.

- Scatter plot (`scatterplot.quorum`)
  - Loads `Data/transposedata.csv`, filters `accuracy = 1`, and creates a scatter plot with a linear regression line.
  - Output: displays a window; exports `Images/scatterplot.html`.

- Box plots (`Boxplot.quorum`)
  - Loads `Data/response.csv` multiple times to plot groups of operator spellings (AND/OR/XOR/NOT/LSHIFT/RSHIFT).
  - Output: by default, the script displays and exports the right-shift plot to `Images/Boxplot/rshiftboxplot.html`. To export the others, uncomment the corresponding `Display(...)` and `Share(...)` lines in the source.

- One-way ANOVA (`oneWayANOVA.quorum`)
  - Iterates over each operator’s transposed dataset (e.g., `Data/individualopdata/andtranspose.csv`).
  - Output: prints the formal ANOVA summary for each group to the console.

- Tukey’s HSD (`Tukey's HSD Multiple Comparison Test.quorum`)
  - Runs Tukey’s HSD pairwise comparisons for each operator using column ranges tailored to the wide-format CSVs in `Data/individualopdata/`.
  - Output: prints formal pairwise comparison summaries to the console.

- Descriptive statistics (`DescriptiveAnalysis.quorum`)
  - For each operator family, iterates across columns in `fulldata.csv` and prints Mean, Standard Deviation, and Variance.
  - Output: printed to the console.

## Outputs

- Console summaries: regression, ANOVA, Tukey HSD, and descriptive statistics print directly to the Quorum output console.
- HTML charts: saved under `Images/`
  - `Images/scatterplot.html`
  - `Images/Boxplot/*.html`

Open these HTML files in your browser to view the plots.

## Reproducibility tips

- Ensure all CSVs remain in their current relative locations under `Data/`.
- Run from the project root via Quorum Studio so relative paths resolve.
- If a plot doesn’t export, check whether the relevant `Share("...")` line is commented out in the source and uncomment as needed.

## Troubleshooting

- File not found errors: confirm you opened and ran `Project/Project.qp` from the project root in Quorum Studio. Paths are relative to the project root.
- Empty plots or missing points: verify `accuracy` filtering (e.g., `accuracy = 1`) matches rows present in `Data/transposedata.csv`.
- Overwriting images: re-running plotting scripts will overwrite the HTML in `Images/`. Copy or rename if you need to preserve versions.

## Contributing

- For small fixes (typos, doc improvements): open a PR.
- For analysis changes: briefly describe the change, affected datasets, and any assumptions.

## License

Specify your license here (e.g., MIT). If you’re publishing this repository, add a `LICENSE` file and update this section accordingly.
