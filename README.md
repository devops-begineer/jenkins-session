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
