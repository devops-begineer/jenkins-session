# Jenkins Interview Preparation

## Q. What is Jenkins?

**Ans:** Jenkins is an open-source automation tool that helps developers automate the process of building, testing, and deploying their code. It actually automates repetitive tasks.

## Q. Does Jenkins support both CI and CD?

**Ans:** Yes.

## Q. What is CI and CD?

**Ans:**

- **CI (Continuous Integration):** The process of automatically triggering builds and tests whenever there is a change in the code repository. It integrates with Git to pull the latest code, integrate changes, and execute the build.

- **CD (Continuous Delivery):** After CI is completed, it takes one step further by automatically deploying every code change that passes all the stages of the pipeline directly to different environments, making it ready for release.

## Q. What is a pipeline in Jenkins?

**Ans:** A pipeline in Jenkins is a series of automated steps that we define to build, test, and deploy the application. Jenkins pipelines are defined in a file called a `Jenkinsfile`. There are two ways to define the pipeline:
1. Declarative Pipeline
2. Scripted Pipeline

## Q. What is the difference between Scripted Pipelines and Declarative Pipelines?

**Ans:**

### Scripted Pipeline
- **Description:** It is flexible and powerful. It is covered with a `node` block.
- **Example:**

    ```groovy
    node {
        try {
            stage('Build') {
                echo 'Building...'
                // Add your build commands here
            }

            stage('Test') {
                echo 'Testing...'
                // Add your test commands here
            }

            stage('Deploy') {
                echo 'Deploying...'
                // Add your deploy commands here
            }
        } catch (Exception e) {
            echo "An error occurred: ${e.message}"
            currentBuild.result = 'FAILURE'
        } finally {
            echo 'This will always run'
            if (currentBuild.result == 'SUCCESS') {
                echo 'Pipeline succeeded'
            } else {
                echo 'Pipeline failed'
            }
        }
    }
    ```

- The `node` block specifies that the pipeline runs on a Jenkins agent.

### Declarative Pipeline
- **Description:** It is more straightforward and structured. It provides predefined, user-friendly syntax and is covered within a `pipeline` block.
- **Example:**

    ```groovy
    pipeline {
        agent any

        stages {
            stage('Build') {
                steps {
                    echo 'Building...'
                    // Add your build commands here
                }
            }

            stage('Test') {
                steps {
                    echo 'Testing...'
                    // Add your test commands here
                }
            }

            stage('Deploy') {
                steps {
                    echo 'Deploying...'
                    // Add your deploy commands here
                }
            }
        }

        post {
            always {
                echo 'This will always run'
            }
            success {
                echo 'This will run only if the pipeline succeeds'
            }
            failure {
                echo 'This will run only if the pipeline fails'
            }
        }
    }
    ```

- The `pipeline` block defines the entire pipeline.
- Inside the `stages` block, there are three stages: Build, Test, and Deploy.
- Each stage has steps that execute specific commands.
- The `post` block defines actions that run based on the pipeline's outcome (`always`, `success`, `failure`).

## Difference Between Scripted and Declarative Pipelines

| Feature                         | Scripted Pipeline                                      | Declarative Pipeline                              |
|---------------------------------|--------------------------------------------------------|---------------------------------------------------|
| **Pipeline Code Validation**    | Code validation happens during pipeline execution. It's like checking for mistakes in a recipe while cooking the dish. | Code validation happens when the pipeline is loaded into Jenkins. Errors are detected upfront before running the pipeline. It's like having someone review your recipe for mistakes before you start cooking. |
| **Restart from Stage**          | Not supported directly.                                | Supported.                                        |
| **Skipping Stages**             | No built-in support for skipping stages. All stages will be executed sequentially. | Supports skipping stages using the `when` directive. Allows conditional skipping of stages based on specified conditions. |

# Triggering Jenkins Jobs

## Q. What are the different ways to trigger a Jenkins job?

**Ans:**

### **Manual Triggering:**
- **Build Now:** Manually trigger a job by clicking the "Build Now" button in the Jenkins UI.

### **Automatic Triggering:**

1. **Source Code Management (SCM) Polling:**
   - Jenkins can automatically trigger jobs by polling the source code repository (e.g., Git) for changes.
   - **Setup:** Check the "Poll SCM" option in the job configuration and specify the schedule using cron-like syntax (e.g., `* * * * *`).
   - **Behavior:** Jenkins will periodically check the repository for any new changes. If changes are detected, the job is triggered; otherwise, nothing happens.

2. **Webhooks from Version Control Systems (e.g., GitHub):**
   - Jenkins can automatically trigger jobs via webhooks configured in your version control system like GitHub.
   - **Setup:** Configure a webhook in GitHub and check the "GitHub Webhook trigger" option under "Build Trigger" in Jenkins.
   - **Behavior:** This option triggers the pipeline job whenever a new commit is made to the GitHub repository.

3. **Scheduled Triggering with Cron Syntax:**
   - Jenkins can automatically trigger jobs based on a defined schedule, regardless of changes in the repository.
   - **Via Build Periodically:**
     - **Setup:** Check the "Build periodically" option and provide a cron schedule (e.g., `H/5 * * * *`).
     - **Behavior:** Jenkins triggers the job at the specified times, without checking for changes in the repository. The job runs on the schedule no matter what.

4. **Jenkins REST API:**
   - Jenkins jobs can be triggered remotely using the Jenkins REST API.
   - **Usage:** This method is useful for integrating Jenkins triggers into other applications or systems.

5. **Triggering from Other Jobs (as part of a pipeline):**
   - Jobs can be triggered as part of a larger Jenkins pipeline, often to coordinate multiple builds or steps.
   - **Setup:** This is typically done within a `Jenkinsfile`, where downstream jobs are triggered as part of the pipeline workflow.

### **Automatic Triggering Detailed Explanation:**

- **Via Webhook:**
  - **Setup:** Configure a webhook in GitHub, and then check the "GitHub Webhook trigger" under the "Build Trigger" option in Jenkins.
  - **Use Case:** This option is used when you want your pipeline job to trigger automatically whenever a new commit is made to the GitHub repository.

- **Via Poll SCM:**
  - **Setup:** Check the "Poll SCM" option and provide a schedule using cron syntax, such as `* * * * *`.
  - **Use Case:** This option is useful when you want Jenkins to check the GitHub repository at regular intervals. If new changes are found, the job triggers; if not, nothing happens.

- **Via Build Periodically:**
  - **Setup:** Check the "Build periodically" option and provide a cron schedule.
  - **Use Case:** This option triggers the job at the specified time intervals, regardless of whether there are new changes in the repository. The job runs according to the schedule, even if the code hasn't changed.

# Common Jenkins Environment Variables

## Q. What are some common Jenkins environment variables?

**Answer:** Common Jenkins environment variables include:

- **`BUILD_ID`**: The unique identifier for the current build.
- **`BUILD_NUMBER`**: The number of the current build.
- **`JOB_NAME`**: The name of the job.
- **`WORKSPACE`**: The path to the workspace directory.
- **`GIT_COMMIT`**: The commit ID of the current Git revision.
- **`JENKINS_HOME`**: The path to Jenkins’ home directory.

