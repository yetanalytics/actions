# yetanalytics/actions

GH Actions for use in Yet Analytics projects

# Usage

## `setup-env` - CI/CD Environment Setup Action

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

## `deploy-clojars` - Clojars Deployment Action

Invoke in order to compile a JAR file that will be deployed to Clojars. The JAR will also be stored as a workflow artifact. The following demonstrates a call to this action with all required inputs:

```yaml
    steps:
      - name: Deploy to clojars
        uses: yetanalytics/actions/deploy-clojars@<tag>
        with:
          artifact-id: 'my-library'
          version: '0.1.0'
          clojars-username: ${{ secrets.CLOJARS_USERNAME }}
          clojars-deploy-token: ${{ secrets.CLOJARS_DEPLOY_TOKEN }}
```

The following is a table of all inputs:

Name | Description
--- | ---
`artifact-id` | The Clojars Artifact ID (i.e. the name of the lib itself). **Required**
`version` | The version string. (By Clojars convention, you should remove any prefixes, e.g. the `v` in `v0.1.0`.) **Required**
`clojars-username` | The Clojars username (should be a GitHub secret). **Required**
`clojars-deploy-token` | The Clojars deploy token (should be a GitHub secret). **Required**
`group-id` | The Clojars Group ID. Defaults to `'com.yetanalytics'`.
`src-dirs` | A quoted array of the source directories. Defaults to `'["src/main"]'`.
`resource-dirs` | A quoted array of the resource directories. Defaults to `'["resources"]'`. Pass `'[]'` if no resource directories exist.
`publish` | Whether or not to actually publish to Clojars. Default `true`; turn off for debugging.

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
