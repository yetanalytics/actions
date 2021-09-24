# yetanalytics/actions

GH Actions for use in Yet Analytics projects

# Usage

## Setup CI/CD Environment

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

* Check out the project
* Install Java
* Install node
* Install Clojure CLI

Providing an environment suitable for testing and building Clojure(Script) projects.

# License

Copyright © 2021 Yet Analytics Inc.

Licensed under the Apache License, Version 2.0.
