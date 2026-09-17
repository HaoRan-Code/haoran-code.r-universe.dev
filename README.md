# haoran-code.r-universe.dev

The R-universe registry behind `https://haoran-code.r-universe.dev`, a CRAN-like repository
that serves the R arm of [scMarkerAgent](https://github.com/HaoRan-Code/scMarkerAgent)
together with `presto`, the differential-expression engine it depends on, which is not on
CRAN. Installing from it needs no GitHub API access and gives prebuilt binaries on Windows
and macOS:

```r
options(repos = c(scmarkeragent = "https://haoran-code.r-universe.dev",
                  CRAN = "https://cloud.r-project.org"))
install.packages("scmarkeragent")
```

`packages.json` lists two packages:

| Package | Source | Why this source |
| --- | --- | --- |
| `scmarkeragent` | `HaoRan-Code/scMarkerAgent`, subdirectory `R/`, default branch | The R arm; rebuilt when that repo moves or this registry entry changes |
| `presto` | `HaoRan-Code/presto`, branch `scmarkeragent` | A fork whose branch is pinned at `a24772a135c7895a8183b007376050556c60a05b` (presto 1.0.0), the commit the released pipeline and the benchmarks were validated with. Upstream master has since changed the tie correction in `wilcoxauc()` (1.1.0); a registry can name a branch or tag but not a commit, hence the fork |

R-universe's own monorepo (`r-universe/haoran-code`) is writeable only by the build
bot. After rewriting history on `scMarkerAgent`, bump `metadata.source_commit` in
`packages.json` (or wait for the hourly scan) so the universe re-clones the new tip.

If presto 1.1.0 is later published on CRAN, `install.packages()` prefers the higher version
and installs CRAN's copy over the pinned 1.0.0. The pipeline still runs, but `wilcoxauc()`
p-values then follow the corrected tie handling and can differ slightly from the
benchmarked release; the pin is to be revisited then.
