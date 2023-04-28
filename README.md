# actions

This directory contains reusable workflows in subdirectory that can be used in different workflows. The reusable workflows are designed to automate repetitive tasks in a software development workflow, such as deploying the application to different environments, checking the code for errors, and generating reports.

## List of reusable workflows
- [benchmarks.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/benchmarks.yml) caller of this workflow is [here](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/benchmark.yaml)
- [history.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/history.yml) caller of this workflow is [here](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/history_HTEC.yaml)
- [miopen-db.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/miopen-db.yml) caller of this workflow is [here](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/miopen_database.yaml)
- [perf-test.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/perf-test.yml) caller of this workflow is [here](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/performance_HTEC.yaml)
- [rocm-release.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/rocm-release.yml) caller of this workflow is [here](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/rocm-image-release_HTEC.yaml)

Each reusable workflow is described in more details [here](https://github.com/migraphx-benchmark/actions/tree/main/.github/workflows)

---
## Graph
![MIGraphX graph](https://github.com/migraphx-benchmark/actions/blob/main/Migraphx.drawio.png)

<p align="center">
  <a href="https://app.diagrams.net/#Hmigraphx-benchmark%2Factions%2Fmain%2FMigraphx.drawio.png">Open graph here</a>
</p>
