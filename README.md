<div align="center">
 <h1>EIC_Misc</h1>
</div>

<!-- badges: start -->
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
<!-- badges: end -->

Miscellaneous functions that might be useful.

A small collection of standalone R helpers that solve problems I hit often enough
to be worth keeping, but not often enough to justify a package. Each file is
self-contained and can be sourced directly:

```r
source("https://raw.githubusercontent.com/EIvimeyCook/EIC_Misc/main/orchaRd_table.R")
```

These are working utilities rather than maintained software.

## Contents

| File | Purpose |
| :--- | :------ |
| [`orchaRd_table.R`](orchaRd_table.R) | Combine a `metafor` results table and an `orchaRd` orchard plot into a single publication-ready figure |

## `orchaRd_table()`

A meta-analysis figure usually wants two things side by side: the orchard plot
showing the distribution of effect sizes, and a table of the model estimates
underneath it. Assembling that by hand each time is tedious. This function takes a
fitted `metafor` model and returns both, already stacked and formatted.

```r
orchaRd_table(model, mod, es, group, terms = NULL, pb = NULL)
```

| Argument | Description |
| :------- | :---------- |
| `model` | A fitted `rma.uni` or `rma.mv` model |
| `mod` | The moderator to summarise. Use `"1"` for an intercept-only model |
| `es` | Effect size label for the plot's x-axis |
| `group` | Grouping variable passed to `orchaRd::mod_results()` |
| `terms` | Optional custom predictor labels; defaults to the model's own term names |
| `pb` | Optional publication-bias components to hold at zero. Estimates are automatically corrected to the point where these equal zero |

Returns a `patchwork` object: a `gt` table above an orchard plot in a 2:6 height
ratio. The table reports estimate, standard error, 95% confidence interval, test
statistic, and *p*-value to three decimal places, with significant rows bolded,
and a source note carrying the tests for residual heterogeneity and for
moderators.

### Example

```r
library(metafor)

mod <- rma.mv(yi, vi, mods = ~ treatment, random = ~ 1 | study_id, data = dat)

orchaRd_table(
  mod,
  mod   = "treatment",
  es    = "Hedges' g",
  group = "study_id",
  terms = c("Intercept", "Treatment: high", "Treatment: low")
)
```

### Dependencies

`orchaRd`, `tidyverse`, `broom`, `patchwork` (≥ 1.3.0, for `wrap_table()`), and
`gt`.

```r
install.packages(c("tidyverse", "broom", "patchwork", "gt", "metafor"))
devtools::install_github("daniel1noble/orchaRd", ref = "main")
```

## Bug reports and contributions

These are personal utilities, but if something here is broken or you have a fix,
please open an issue at <https://github.com/EIvimeyCook/EIC_Misc/issues>.

## Contact

Edward R. Ivimey-Cook — <e.ivimeycook@gmail.com> —
[ORCID 0000-0003-4910-0443](https://orcid.org/0000-0003-4910-0443)

## Citation

> Ivimey-Cook, E. R. (2026). *EIC_Misc: Miscellaneous R functions*.
> <https://github.com/EIvimeyCook/EIC_Misc>

A machine-readable [`CITATION.cff`](CITATION.cff) is included, so GitHub's
"Cite this repository" button gives formatted APA and BibTeX.

## License

Released under the [MIT License](LICENSE).
