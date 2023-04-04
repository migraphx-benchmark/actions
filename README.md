Needs : [benchmark-utils](https://github.com/migraphx-benchmark/benchmark-utils), [AMDMIGraphX](https://github.com/migraphx-benchmark/AMDMIGraphX)
-------------------------------------------------------------------------------------------
## Perf-test.yml

Performance test  will be trigered by caller [workflow](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/performance.yaml). This workflow will run docker build image, run performance test for each model, collect all results, make graph, do the backup (after nightly run or after approved pull request) depending of the organization will be pushed to repository regarding to HTEC or AMD.


## history.yml 
Will be triged on workflow dispatch event from caller [workflow](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/history.yaml). On dispatch event window enter date range and results with graph will be pushed to [repository](https://github.com/migraphx-benchmark/migraphx-reports).



## benchmark.yml 
Will be triged on workflow dispatch event from caller [workflow](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/benchmark.yaml). Check if migraphx image exist, if not build new docker image an run benchmark test. New results will be saved as "migraphx-compare-{date}.xlsx"

## rocm-release.yml
Will be triged on workflow dispatch event from caller [workflow](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/rocm-image-release.yaml). On dispatch event window enter ROCm version, if image exist already nothing will happen, but also you have overwrite button to create same image again.

## miopen-db.yml
Will be triged on workflow dispatch event from caller [workflow](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/miopen_database.yaml)
