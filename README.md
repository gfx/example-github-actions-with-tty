# Hack to create TTY for GitHub Actions runners

As of December 2020, GitHub Actions do not create TTY for their runnders, as the following issue has reported:

https://github.com/actions/runner/issues/241

For a wokaround in Linux, `script(1)` can solve this problem like this:

```yaml
# ...

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Run a multi-line script
        shell: 'script -q -e -c "bash {0}"'
        run: |
          perl -E 'say "Is STDOUT a TTY?: ", -t STDOUT ? "yes" : "no"'
```

where `shell: 'script -q -e -c "bash {0}"'` is the trick.
