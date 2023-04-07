# Workflows

## `benchmarks.yml`

<p>
This is a GitHub Actions workflow that benchmarks the performance of MiGraphX, a machine learning framework,<br> using the Open Neural Network Exchange Runtime (ONNX Runtime) framework.
</p>

- ### Trigger
> The workflow will be triggered on workflow dispatch event from caller workflow 
[benchmark.yaml](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/benchmark.yaml) 

- ### Input Parameters
The workflow uses the following input parameters:
rocm_version: 
> - `rocm_version`: the ROCm release version, which is a required parameter

> - `script_repo`: the script repository, which is an optional parameter

> - `result_path`: the path where the result will be stored, which is an optional parameter

> - `result_repo`: the repository where the result will be stored, which is an optional parameter

- ### Environment Variables
The workflow uses the following environment variables:

>- `SCRIPT_PATH`: This environment variable specifies the path to the script repository. <br> It is set to `benchmark-utils`.

>- `RESULT_PATH`: This environment variable specifies the path to the result repository. <br> It is set to `comparison-results`.

- ### Jobs
The following jobs are executed in the workflow:
> - `check_image_version`: This job checks if a new Docker image needs to be built.<br> It runs on a self-hosted machine and sets the output parameter `image` to `true` if a new image needs to be built.<br> This output is used by the next job, `build_image`.

> - `build_image`: This job builds a new Docker image if the `check_image_version` job indicated that one is needed.<br> It runs on a self-hosted machine and depends on the `check_image_version` job. <br>If a new image needs to be built, it checks out the code, checks out the benchmark utilities, and then builds the Docker image. <br>The image is tagged with the current date, and the job sets no outputs.

> - `run_benchmark`: This job runs the benchmark. It runs on a self-hosted machine and depends on the `build_image` job.<br> It exports the start time of the benchmark as an output parameter, `result_time_start`.<br>It then executes the benchmark script in the Docker container and deletes old images and containers. <br>The script takes several arguments, including the result path, which is an input parameter. The job sets no other outputs.

> - `git_push_result`: This job pushes the benchmark results to a GitHub repository. It runs on a self-hosted machine and depends on the `run_benchmark` job.<br> It checks out the results repository, executes two Python scripts to generate a report, and pushes the report to the results repository.<br> The job sets no outputs.

For more details, please refer to the [benchmarks.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/benchmarks.yml) file in the repository.

---
## `history.yml`

TODO

---

## `miopen-db.yml`

TODO

---

## `perf-test.yml`

TODO

---

## `rocm-release.yml`

TODO

---
