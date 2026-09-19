# haoran-code.r-universe.dev

R packages for scMarkerAgent and its differential-expression dependency `presto`.

```r
options(repos = c(scmarkeragent = "https://haoran-code.r-universe.dev",
                  CRAN = "https://cloud.r-project.org"))
install.packages("scmarkeragent")
```

`packages.json` tracks `HaoRan-Code/scMarkerAgent` on its default branch, with the
R package in `R/`. The `presto` entry tracks `HaoRan-Code/presto` on the
`scmarkeragent` branch. R-universe checks repositories for changes hourly and
builds updated packages automatically. No custom source-commit metadata is needed.

Check build results in [GitHub Actions](https://github.com/r-universe/haoran-code/actions)
and the installed package source in the
[package API](https://haoran-code.r-universe.dev/api/packages/scmarkeragent).
Publication is complete when `RemoteSha` matches the intended source commit;
changing registry metadata alone does not demonstrate that a build started.
