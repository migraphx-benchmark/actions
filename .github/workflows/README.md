# Workflows

## `benchmarks.yml`

<p>
This is a GitHub Actions workflow that benchmarks the performance of MiGraphX, a machine learning framework, using the Open Neural Network Exchange Runtime (ONNX Runtime) framework.
</p>

- ## Trigger
> The workflow will be triggered on workflow dispatch event from caller workflow 
[benchmark.yaml](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/benchmark.yaml) 

- ## Input Parameters
The workflow uses the following input parameters:

> - `rocm_version`: The ROCm release version, which is a required parameter.

> - `script_repo`: The script repository, which is an optional parameter.

> - `result_path`: The path where the result will be stored, which is an optional parameter.

> - `result_repo`: The repository where the result will be stored, which is an optional parameter.

- ## Environment Variables
The workflow uses the following environment variables:

>- `SCRIPT_PATH`: The path to the script repository.

>- `RESULT_PATH`: The path to the result repository.

- ## Jobs
The following jobs are executed in the workflow:
> - `check_image_version`: This job checks if a new Docker image needs to be built. It runs on a self-hosted machine and sets the output parameter `image` to `true` if a new image needs to be built. This output is used by the next job, `build_image`.

> - `build_image`: This job builds a new Docker image if the `check_image_version` job indicated that one is needed. It runs on a self-hosted machine and depends on the `check_image_version` job. If a new image needs to be built, it checks out the code, checks out the benchmark utilities, and then builds the Docker image. The image is tagged with the current date, and the job sets no outputs.

> - `run_benchmark`: This job runs the benchmark. It runs on a self-hosted machine and depends on the `build_image` job. It exports the start time of the benchmark as an output parameter, `result_time_start`. It then executes the benchmark script in the Docker container and deletes old images and containers. The script takes several arguments, including the result path, which is an input parameter. The job sets no other outputs.

> - `git_push_result`: This job pushes the benchmark results to a GitHub repository. It runs on a self-hosted machine and depends on the `run_benchmark` job. It checks out the results repository, executes two Python scripts to generate a report, and pushes the report to the results repository. The job sets no outputs.

For more details, please refer to the [benchmarks.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/benchmarks.yml) file in the repository.

---
## `history.yml`

<p>
This workflow analyzes the historical performance of the MIGraphX project by running a Python script that produces a report between two given dates. The results are uploaded to a Github repository and a link to the report is provided. The workflow requires several input parameters and access to Github credentials as secrets.
</p>

- ## Trigger
> The workflow will be triggered on workflow dispatch event from caller workflow 
[history_HTEC.yaml](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/history_HTEC.yaml) 

- ## Input Parameters
The workflow uses the following input parameters:
> - `start_date`: The start date for the results analysis, which is a required parameter.

> - `end_date`: The end date for the results analysis, which is a required parameter.

> - `history_repo`: The repository where the history will be stored, which is a required parameter.

> - `benchmark_utils_repo`: The repository where the benchmark utilities are stored, which is a required parameter.

> - `organization`: The organization based on which the location of files will be different, which is a required parameter.

- ## Environment Variables
The workflow uses the following environment variables:

>- `TEST_RESULTS_PATH`: The path to the test results directory.

>- `UTILS_DIR`: The directory where the benchmark utilities are stored.

>- `REPORTS_DIR`: The directory where the reports will be stored.

>- `REPORTS_PATH`: The path to the reports directory.

- ## Jobs
The workflow has a single job named `performance_test`. The following steps are executed in this job:
> - `Checkout code`: This step checks out the code for the MIGraphX project.

> - `Checkout utils`: This step checks out the benchmark utilities repository specified by the user in the inputs. The repository is checked out to the `UTILS_DIR` directory using the `path` parameter.

> - `Checkout report's repo`: This step checks out the repository where the historical analysis report will be stored. The repository is checked out to the `REPORTS_DIR` directory using the `path` parameter.

> - `Run history script`: This step runs a Python script named `history.py` located in the `UTILS_DIR/scripts/` directory. The script takes the start and end dates specified by the user in the inputs, and the paths to the test results and reports directories specified in the environment variables, as command line arguments. The script generates a historical analysis report for the specified time period.

> - `Upload history results`: This step copies the generated historical analysis report to the `REPORTS_DIR` directory and adds it to the Git repository. It then commits the changes with a message specifying the time period of the report and pushes the changes to the remote repository.

> - `Get link to results repository`: This step prints a link to the historical analysis report repository specified in the inputs, which can be used to access the report.

For more details, please refer to the [history.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/history.yml) file in the repository.

---

## `miopen-db.yml`

<p>
This workflow generates an MIOPEN database by performing tuning tests and saving the results to a database.
</p>

- ## Trigger
> The workflow will be triggered on workflow dispatch event from caller workflow 
[miopen_database.yaml](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/miopen_database.yaml) 

- ## Input Parameters
The workflow uses the following input parameters:

> - `rocm_release`: The version of ROCm release, which is a required parameter.

> - `miopen_db_repo`: The MIOpen Database repository, which is an optional parameter.

> - `script_repo`: The script repository, which is an optional parameter.

> - `saved_models_path`: The path to the saved models, which is an optional parameter.

> - `test_results_dir`: The path to the test results directory, which is an optional parameter.

- ## Environment Variables
The workflow uses the following environment variables:

>- `ROCM_VERSION`: The version of the ROCm release.

>- `MIOPEN_PATH`: The path to the MIOpen database repository.

>- `SCRIPT_PATH`: The path to the script repository.

- ## Jobs
The following jobs are executed in the workflow:
> - `check_gpu_name`: This job checks the name of the GPU being used and sets the output `gpu_name` to be used in subsequent jobs.

> - `check_image_version`: This job checks if the Docker image `rocm-migraphx` for the specified `rocm_release` exists and sets the output `image` accordingly.

> - `check_database`: This job checks if the database path exists for the specified GPU and ROCm release version, and sets the output `database` accordingly.

> - `create_database`: This job creates the MIOpen database by performing tuning using `run_perf_mev.sh` script inside a Docker container with the necessary environment variables, mounted volumes and devices, and saves the resulting database to a directory on the host machine.

> - `push_database`: This job pushes the created MIOpen database to the specified GitHub repository using Git commands, after creating a subdirectory with the name of the GPU in the version-specific directory.

For more details, please refer to the [miopen-db.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/miopen-db.yml) file in the repository.


---

## `perf-test.yml`

<p>
Overall, this workflow is designed for running performance tests on the MIGraphX library and saving the results to a specified repository.
</p>

- ## Trigger
> The workflow will be triggered on workflow dispatch event from caller workflow 
[performance_HTEC.yaml](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/performance_HTEC.yaml) 

- ## Input Parameters
The workflow uses the following input parameters:
> - `rocm_release`: The version of ROCm release, which is a required parameter.

> - `performance_reports_repo`: The repository where the performance reports will be stored, which is a required parameter.

> - `benchmark_utils_repo`: The repository where the benchmark utilities are stored, which is a required parameter.

> - `organization`: The organization based on which the location of files will be different, which is a required parameter.

> - `result_number`: The number of the last results, which is a required parameter.

> - `model_timeout`: If a model in the performance test script passes this threshold, it will be skipped. It's also required parameter.

> - `flags`: The flags used for performance testing, such as "-m" for Max value, "-s" for Std dev, or "-r 'path'" for the threshold file. It's also required parameter.

- ## Environment Variables
The workflow uses the following environment variables:

>- `UTILS_DIR`: The directory where the benchmark utilities are stored.

>- `REPORTS_DIR`: The directory where the performance test reports will be saved.

>- `DOCKERBASE`: The base Docker image used to run the performance tests.

>- `MIOPENTUNE`: The path to the MiOpenTune directory.

>- `MAIL_TO`: The email address where the performance test results will be sent.

>- `MAIL_CC`: The email addresses that will receive a copy of the performance test results.

>- `MAIL_FROM`: The sender of the email that contains the performance test results.

>- `MAIL_SUBJECT`: The subject of the email that contains the performance test results.

>- `MAIL_BODY`: The body of the email that contains the performance test results.

>- `PR_ID`: The ID of the pull request that triggered the workflow.

>- `BRANCH_NAME`: The name of the branch that the pull request is targeting.

>- `REPORTS_PATH`: The path where the performance test reports will be saved.

>- `TEST_RESULTS_PATH`: The path where the test results will be saved.

>- `MIGRAPHX_PATH`: The path where the MIGraphX project is located.

>- `PERFORMANCE_DIR`: The directory where the previous performance test results are stored.

>- `PERFORMANCE_DIR_HTEC`: The directory where the previous performance test results are stored for the HTEC organization.

- ## Jobs
The workflow has a single job named `performance_test`. The following steps are executed in this job:
> - `Update Mailing list based on organization`: If the organization is HTEC, update the mailing list.

> - `Update PR env`: If the event name is `pull_request` update the environment variables related to the pull request.

> - `Checkout code`: Check out the code of the repository.

> - `Checkout utils`: Check out the benchmark utilities repository and place it in the UTILS_DIR environment variable.

> - `Get git SHA`: Get the git SHA for the current commit and store it in the git_sha output variable.

> - `Docker build`: Build a Docker container based on the Daily.Dockerfile in the benchmark utilities repository.

> - `Run performance tests`: Run the performance tests inside the Docker container.
### Comment out step for now:
```
 - Delete old images/containers: This step deletes old Docker images and containers based on a reference and prunes unused images.
```
> - `Checkout report's repo`: This step checks out a Git repository for performance reports using the `actions/checkout` action and sets the path for the repository.

> - `Execute report script`: This step executes a Python script for generating performance reports using the `run` command. The script generates a report, copies the results to a directory, commits the changes, and pushes them to the Git repository checked out in the previous step.

> - `Execute comment script`: This step executes a Python script for creating a comment on a pull request using the `run` command. The script takes parameters such as test results path, result number, and flags.

> - `Create a comment on PR`: This step creates a sticky comment on a pull request using the `marocchino/sticky-pull-request-comment` action. The comment includes a header and the path for the comment script.

> - `Get latest accuracy results`: This step gets the latest accuracy results by changing the working directory and using a command to list the accuracy results and select the latest one.

> - `Create accuracy comment on PR`: This step creates a sticky comment on a pull request for accuracy results using the `marocchino/sticky-pull-request-comment` action. The comment includes a header and the path for the accuracy results.

> - `Get latest report`: This step gets the latest performance report by changing the working directory, listing the reports, and selecting the latest one.

> - `Send mail`: This step sends an email using the `dawidd6/action-send-mail` action. The email includes the subject, recipient, sender, body, and attachment of the latest performance report.

> - `Checkout for backup`: This step checks out a Git repository for performance backup using the `actions/checkout` action and sets the path for the repository.

> - `Checkout for Htec-backup`: This step checks out a Git repository for Htec backup using the `actions/checkout` action and sets the path for the repository.

> - `Backup`: This step creates a backup of performance results if a pull request is closed and merged, or if a scheduled event occurs. It copies the performance results to a directory based on the organization name, adds the changes to Git, and pushes the commit.

> - `Clean merged PR data`: This step runs a script in a Docker container to clean up data after a pull request is merged. It mounts directories and sets a working directory before executing the script.

> - `Clean closed PR data`: This step is similar to the previous step, but it runs a script to clean up data after a pull request is closed without being merged.



For more details, please refer to the [perf-test.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/perf-test.yml) file in the repository.


---

## `rocm-release.yml`

<p>
This workflow automates the process of building a Docker image for the ROCm (Radeon Open Compute) software stack developed and maintained by Advanced Micro Devices (AMD) Corporation. ROCm is an AMD software stack designed for high-performance computing (HPC) and machine learning (ML) workloads, with a focus on providing support for AMD hardware, including GPUs and CPUs.
</p>

- ## Trigger
> The workflow will be triggered on workflow dispatch event from caller workflow 
[rocm-image-release_HTEC.yaml](https://github.com/migraphx-benchmark/AMDMIGraphX/blob/develop/.github/workflows/rocm-image-release_HTEC.yaml) 

- ## Input Parameters
The workflow uses the following input parameters:
> - `rocm_release`: The version of the ROCm software stack to be used in the Docker build, which is a required parameter.

> - `benchmark-utils_repo`: The repository for the benchmark utilities that will be used in the build, which is a required parameter.

> - `organization`: The organization that the Docker image is being built for, which is a required parameter.

> - `base_image`: The base image for the ROCm Docker build, which is a required parameter.

> - `docker_image`: The name of the Docker image that will be created, which is a required parameter.

> - `build_navi`: The build number for the Navi GPU architecture, which is a required parameter.

> - `overwrite`: A flag to determine whether to overwrite the Docker image if it already exists, which is a required parameter.

- ## Environment Variables
The workflow uses only one environment variable:

>- `UTILS_DIR`: The directory where the benchmark utilities repository will be checked out during the Docker build process.

- ## Jobs
The following jobs are executed in the workflow:
> - `check_image_version`: This job checks whether the specified Docker image already exists. If it does, the job checks the `overwrite` flag to determine whether to delete the existing image and create a new one. This job outputs a boolean value indicating whether a new image needs to be built.

> - `build_image`: This job builds the ROCm Docker image if the `check_image_version` job determined that a new image needs to be built. This job checks out the benchmark utilities repository, sets environment variables based on the input parameters, and runs a script to build the Docker image.

For more details, please refer to the [rocm-release.yml](https://github.com/migraphx-benchmark/actions/blob/main/.github/workflows/rocm-release.yml) file in the repository.


---
