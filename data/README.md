# Data access and input format

The experimental and analytical data used with this notebook are publicly available on Zenodo:

https://doi.org/10.5281/zenodo.22086423

For the response-surface analysis, download:

```text
data_processed/doe/spata_doe_formatted_results.xlsx
```

Place the workbook in the notebook's working directory or provide its full path in `EXCEL_FILENAME` in section 4 of the notebook.

The input table must contain columns corresponding to:

- donor loading (equivalents);
- acceptor concentration (mM);
- PLP concentration (mM);
- enzyme loading (mg mL^-1); and
- response, currently yield or conversion (%).

The notebook maps responses to the generated design using the physical factor values. Preserve all replicated center-point rows and ensure that column names and units match the assignments in section 4 before fitting the model.

Raw HPAEC-PAD chromatograms and validation experiments are available in the same Zenodo record but are not required to run the response-surface model.

