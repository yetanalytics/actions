# yetanalytics/actions

GH Actions for use in Yet Analytics projects

# Usage

## `setup-env` - Setup CI/CD Environment

Invoke as the first step in a workflow run:

``` yaml

    steps:
      - name: Setup CI Environment
        uses: yetanalytics/actions/setup-env@<tag>
        with:
            java-version: '11'
            java-distribution: 'temurin'
            node-version: '14'
            clojure-version: '1.10.3.943'

```

Default options are shown, they may be omitted.

Will do the following:

* ~~Check out the project~~ post v0, the calling workflow is expected to do this.
* Install Java
* Install node
* Install Clojure CLI

Providing an environment suitable for testing and building Clojure(Script) projects.

## `nvd-scan` - Reusable Vulnerability Scanner Workflow:

Invoke as a workflow job:

``` yaml
...
  nvd_scan:
    uses: yetanalytics/actions/.github/workflows/nvd-scan.yml@<tag>
    with:
      classpath-command: 'clojure -Spath -A:any:alias' # include aliases you want to check or omit the input
      nvd-clojure-version: '1.9.0' # default

  docker:
    needs:
      - nvd_scan # prevents docker from running if scan fails
...

```

Will attempt to run an OWASP dep check via [nvd-clojure](https://github.com/rm-hull/nvd-clojure/blob/master/README.md). If any CVEs are found it will fail. Reports are available as `<sha>-nvd-report`

# License

Copyright © 2021 Yet Analytics Inc.

Licensed under the Apache License, Version 2.0.
