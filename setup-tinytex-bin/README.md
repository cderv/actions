# setup-tinytex-bin

This action sets up TinyTeX by downloading the last release from [tinytex-releases](https://github.com/yihui/tinytex-releases)

This actions works on Linux, Windows and MacOS.

> **This actions live only in this fork in a branch for now !**

# Usage

See [action.yml](action.yml)

Basic:

```yaml
steps:
- uses: cderv/actions/setup-tinytex-bin@tinytex-bin
  with:
    # The directory where to put the binary - will be in $RUNNER_TEMP/bin by default
    directory: $GITHUB_WORKSPACE/.bin 
- run: tlmgr --version
```

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)

# Contributions

Contributions are welcome!
