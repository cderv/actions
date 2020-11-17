# setup-pandoc-nightly

This action sets up pandoc devel which is nighlty build in https://github.com/jgm/pandoc/actions?query=workflow%3ANightly

It will download the last nighlty version built, unzip it in a directory for the current job and add it to the PATH. 

This actions works on Linux, Windows and MacOS.

> **This actions live only in this fork in a branch for now !**

# Usage

See [action.yml](action.yml)

Basic:

```yaml
steps:
- uses: cderv/actions/setup-pandoc-nightly@nightly-pandoc
  with:
    # The directory where to put the pandoc binary - will be in $RUNNER_TEMP by default
    directory: $GITHUB_WORKSPACE/.bin 
- run: echo "# Test" | pandoc -t html
```

In a workflow with both Pandoc release and devel, you will need to use the action on conditional. Here is an example for R package:

```yaml
jobs:
  pandoc-version:
    runs-on: ${{ matrix.config.os }}
    name: ${{ matrix.config.os }} (${{ matrix.config.r }}) [Pandoc ${{matrix.config.pandoc}}]
    strategy:
      fail-fast: false
      matrix:
        config:
          - {os: ubuntu-20.04,   pandoc: '2.11',    r: 'release', rspm: "https://packagemanager.rstudio.com/cran/__linux__/focal/latest"}
          - {os: ubuntu-20.04,   pandoc: 'devel',   r: 'release', rspm: "https://packagemanager.rstudio.com/cran/__linux__/focal/latest"}
    env: 
      RSPM: ${{ matrix.config.rspm }}
    steps:
      - uses: r-lib/actions/setup-r@master
        with:
          r-version: ${{ matrix.config.r }}
      - uses: r-lib/actions/setup-pandoc@v1
        if: matrix.config.pandoc != 'devel'
        with:
          pandoc-version: ${{ matrix.config.pandoc }}
      - uses: cderv/actions/setup-pandoc-nightly@nightly-pandoc
        if: matrix.config.pandoc == 'devel'
      - name: Install deps
        run: install.packages("rmarkdown")
        shell: Rscript {0}
      - name: Check Pandoc version with R
        run: rmarkdown::find_pandoc()
        shell: Rscript {0}
```

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)

# Contributions

Contributions are welcome!
