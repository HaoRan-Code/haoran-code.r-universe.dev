# R-universe distribution of the R arm

Goal: `install.packages("scmarkeragent")` from `https://haoran-code.r-universe.dev` installs
the R arm together with `presto` (not on CRAN), as prebuilt binaries, without touching the
GitHub API. R-universe rebuilds automatically on every push to `main` of scMarkerAgent.

`packages.json` in this directory is the registry the universe reads. It lists the R arm
(the `R/` subdirectory of the scMarkerAgent repository) and a fork of presto whose
`scmarkeragent` branch is pinned at commit `a24772a135c7895a8183b007376050556c60a05b`, the
version the released pipeline and the benchmarks were validated with. presto's own master
is 29 commits ahead (version 1.1.0, with a changed tie correction in `wilcoxauc()`), and
`packages.json` can name a branch or tag but not a commit, hence the fork.

## Steps on GitHub (account owner; done in the web UI)

1. Create a public repository named exactly `haoran-code.r-universe.dev` under the
   `HaoRan-Code` account (empty; no README needed).
2. Fork `https://github.com/immunogenomics/presto` into `HaoRan-Code/presto`
   (the Fork button; keep the name `presto`).
3. Install the R-universe GitHub App on the `HaoRan-Code` account:
   `https://github.com/apps/r-universe` (choose "All repositories").

## Steps from this machine (after 1 and 2 exist)

```bash
# registry
cd /mnt/workdir/cellmarker/0.nature_prepare/r-universe
git init -q && git add packages.json && git commit -q -m "Register scmarkeragent and the pinned presto"
git branch -M main && git remote add origin git@github.com:HaoRan-Code/haoran-code.r-universe.dev.git
git push -u origin main

# pinned presto branch in the fork
git clone -q https://github.com/immunogenomics/presto.git /tmp/presto_pin && cd /tmp/presto_pin
git checkout -q -b scmarkeragent a24772a135c7895a8183b007376050556c60a05b
git push git@github.com:HaoRan-Code/presto.git scmarkeragent
```

The first build appears within about an hour at `https://haoran-code.r-universe.dev`
(build log: `https://github.com/r-universe/haoran-code/actions`). Check with:

```r
options(repos = c(scmarkeragent = "https://haoran-code.r-universe.dev", CRAN = "https://cloud.r-project.org"))
available.packages()[c("scmarkeragent", "presto"), c("Version", "Repository")]
install.packages("scmarkeragent")
```

## Known consequence

If presto 1.1.0 is later published on CRAN, `install.packages()` prefers the higher version
and installs CRAN's 1.1.0 over the universe's pinned 1.0.0. The pipeline still runs (padj
is recomputed by the pipeline itself), but `wilcoxauc()` p-values then follow the corrected
tie handling and can differ slightly from the benchmarked release. Revisit the pin then.
