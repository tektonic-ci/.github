# tektonic

**Define Tekton CI/CD pipelines as strongly-typed TypeScript.** Tektonic is a
[cdk8s](https://cdk8s.io/)-based library for composing params, workspaces, tasks and pipelines
as real code, then synthesizing in-repo [Pipelines as Code](https://pipelinesascode.tekton.dev/)
artifacts that the PAC operator runs straight from your repository. The output is plain Tekton
and PAC YAML, with no runtime dependency on tektonic.

## Packages

| Package | Repo | What it is |
|---|---|---|
| [`@tektonic-ci/core`](https://www.npmjs.com/package/@tektonic-ci/core) | [core](https://github.com/tektonic-ci/core) | The library: primitives, pipelines, PAC/Tekton synthesis, the `tektonic` CLI, and the PVC cache backend |
| [`@tektonic-ci/cache-gcs`](https://www.npmjs.com/package/@tektonic-ci/cache-gcs) | [cache-gcs](https://github.com/tektonic-ci/cache-gcs) | `gcs({ bucket })` and `gcsArtifacts({ bucket })` — cache archives and pipeline artifacts in a Google Cloud Storage bucket |
| [`@tektonic-ci/reporter-github`](https://www.npmjs.com/package/@tektonic-ci/reporter-github) | [reporter-github](https://github.com/tektonic-ci/reporter-github) | `GitHubStatusReporter` — per-task GitHub commit statuses |

Each provider takes `@tektonic-ci/core` as a peer dependency and versions independently. Install
only what you use:

```bash
npm install @tektonic-ci/core
npm install @tektonic-ci/cache-gcs @tektonic-ci/reporter-github   # optional
```

New here? Start with [Getting started](https://github.com/tektonic-ci/core/blob/main/docs/getting-started.md).

## Write your own provider

Caching, status reporting and artifact storage are strategy interfaces in core, and jobs ship as
ordinary npm packages. The two providers above live in repos of their own and build only
against core's published exports, from npm, the way a third-party provider would. Anything they
can do, your package can too:

- [Job libraries](https://github.com/tektonic-ci/core/blob/main/docs/job-libraries.md) — reusable tasks and actions
- [Status reporters](https://github.com/tektonic-ci/core/blob/main/docs/status-reporters.md) — report task results to a forge
- [Cache backends](https://github.com/tektonic-ci/core/blob/main/docs/cache-backends.md) — store cache archives anywhere
- [The conformance kit](https://github.com/tektonic-ci/core/blob/main/docs/testing.md#testing-a-provider-the-conformance-kit) — `@tektonic-ci/core/testing` checks a provider against the contract
