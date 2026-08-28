# Design of Experiments for SpATA-Catalyzed Carbohydrate Amination

This repository provides a reproducible Jupyter/Google Colab workflow for multivariable optimization of a biocatalytic reaction. It was developed for the SpATA-catalyzed amination of C6-oxidized galactose and uses a face-centered central composite design to determine how donor, acceptor, cofactor, and enzyme loadings influence conversion.

The code connects experimental design, statistical interpretation, and practical optimization in one annotated notebook. Although the included configuration reproduces the SpATA study, the core workflow can be adapted to other biocatalytic reactions with continuous experimental factors and a quantitative response.

The workflow:

- generates and randomizes the experimental design;
- imports measured conversion data;
- fits a quadratic response-surface model by ordinary least squares;
- performs Type II ANOVA, lack-of-fit analysis, and model diagnostics;
- visualizes main effects, interactions, response surfaces, and contours; and
- identifies conversion, titer, and reagent-efficient operating points.

## Repository contents

```text
.
├── notebooks/
│   └── spata_doe_response_surface_optimization.ipynb
├── data/
│   └── README.md
├── .gitignore
├── CITATION.cff
├── LICENSE
├── README.md
└── requirements.txt
```

The notebook is a clean GitHub copy with saved outputs removed. Its code cells are unchanged from the annotated analysis notebook; the explanatory Markdown cells identify where users can enter data and change experimental parameters.

## Data

The experimental data are archived separately on Zenodo:

**Gordijo, J. S.; Feng, X.; Master, E. R. Dataset for Amine Donor Selection and Multivariable Optimization in Transaminase-Catalyzed Amino Sugar Synthesis.**  
https://doi.org/10.5281/zenodo.22086423

See [`data/README.md`](data/README.md) for the input workbook and expected columns. Raw analytical data are not duplicated in this code repository.

## Quick start

### Google Colab

1. Upload `notebooks/spata_doe_response_surface_optimization.ipynb` to Google Colab.
2. Run the setup cell.
3. In section 2, review the factor ranges, response name, and number of center points.
4. In section 4, set `EXCEL_FILENAME` to the downloaded results workbook.
5. Run the remaining cells from top to bottom.

### Local installation

Python 3.12 is recommended.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab notebooks/spata_doe_response_surface_optimization.ipynb
```

On Windows, activate the environment with `.venv\Scripts\activate`.

## Main user inputs

The notebook marks the principal settings explicitly:

- `FACTORS`: low and high values, units, and linear or logarithmic scaling;
- `RESPONSE_NAME`: measured response, currently `Yield (%)`;
- `N_CENTER`: number of replicated center points;
- `EXCEL_FILENAME`: input workbook containing measured responses; and
- optimization settings for yield, titer, and multi-objective desirability.

The current study evaluates donor equivalents, acceptor concentration, pyridoxal 5'-phosphate concentration, and enzyme loading.

## Adapting the workflow to other biocatalytic reactions

The workflow can be used as a template for optimizing other enzyme-catalyzed reactions, provided that the experimental factors are quantitative and can be studied over defined ranges. Examples include substrate concentration, cosubstrate loading, enzyme loading, cofactor concentration, pH, temperature, or reaction time.

1. **Define the response.** Set `RESPONSE_NAME` to the measured outcome, such as conversion, yield, productivity, selectivity, or product concentration. If the response is not a percentage, update plots and desirability functions that assume a 0-100 scale.
2. **Define the factors and ranges.** Replace the entries in `FACTORS` with the variables, limits, units, and scaling appropriate to the new reaction. Use logarithmic scaling only when scientifically justified.
3. **Choose center-point replication.** Set `N_CENTER` to provide an estimate of pure experimental error and enable lack-of-fit testing. The appropriate number depends on expected variability and available experimental resources.
4. **Generate and randomize the design.** Run section 3 to create a new face-centered central composite design. Conduct the experiments in the generated randomized order where practical.
5. **Map the experimental data.** In section 4, change `EXCEL_FILENAME`, the input column assignments, and `merge_cols` so that they match the new factor names and response column exactly.
6. **Review reaction-specific controls.** Replace the current no-enzyme and no-acceptor controls with controls appropriate to the new system. Keep controls outside the response-surface model when they fall outside the defined design space, but report them separately.
7. **Update the optimization objectives.** The optional titer and economic-optimization sections currently refer to `Donor`, `Acceptor`, `PLP`, and `Enzyme`. Rename these variables and revise the equations, bounds, weights, and minimum-response criteria for the new process.
8. **Evaluate the model before optimization.** Inspect residuals, influential observations, adjusted and predicted R-squared values, and lack of fit. Do not interpret an optimum when the quadratic model is inadequate.
9. **Validate experimentally.** Perform independent reactions at the predicted optimum and compare measured and predicted responses. The model should not be used to extrapolate beyond the tested factor ranges.

This face-centered design is intended primarily for continuous factors. Categorical variables, such as enzyme identity, immobilization method, or solvent class, require an appropriate categorical or mixed-factor design rather than being treated as arbitrary numerical levels.

## Statistical analysis

The notebook fits a full second-order response-surface model using ordinary least squares. It reports Type II sums of squares, ANOVA F-tests, two-sided tests of regression coefficients, model-fit statistics, residual diagnostics, and lack of fit. Statistical significance is defined as `p < 0.05`.

The generated design uses a fixed random seed of `42`. Prespecified no-enzyme and no-acceptor controls are retained in the dataset but excluded from response-surface model fitting because they lie outside the modeled design space.

## Reproducibility notes

The saved Colab outputs recorded Python 3.12, NumPy 2.0.2, SciPy 1.16.3, Matplotlib 3.10.0, and pyDOE3 1.6.2. Exact versions of some packages were not recorded in the original session; `requirements.txt` therefore uses compatible version ranges for those packages.

The notebook generates design matrices, verification tables, statistical summaries, and plots in the current working directory. Generated outputs and downloaded data are excluded from version control by `.gitignore`.

## Citation

Please cite the associated Zenodo dataset and the related research article when it becomes available. Citation metadata for this repository are provided in [`CITATION.cff`](CITATION.cff).

## License

A software license has not yet been selected. Before making the repository public, the authors should choose a code license and replace [`LICENSE_SELECTION_REQUIRED.md`](LICENSE_SELECTION_REQUIRED.md) with the corresponding license text. MIT or BSD-3-Clause are common permissive choices for research code. The Zenodo data may use a separate data license.
