# GitHub Actions for publishing Docker images

In the .github/workfkows directory, there is a file called publish.yml. This file contains the stub of a GitHub Actions workflow that tests a minimal node.js app, builds a Docker image and publishes it to ghcr.io (GitHub Container Registry). Implement the missing parts of the workflow.

## Complete the run_tests job

The *run_tests* job should run unit tests using npm. Add the following steps to the job.

### 1. Set up Node.js
Use the [action/setup-node](https://github.com/actions/setup-node) action to configure Node.js version 22 in the run environment. Use the "uses:" directive to reference the action. See action's documentation for usage details.

### 2. Install dependencies
Run `npm ci` using the "run:" directive.

### 3. Run tests
Run `npm test` using the "run:" directive.

### 4. Store the code coverage report
Use the [actions/upload-artifact](https://github.com/actions/upload-artifact?tab=readme-ov-file#examples) action to store the code coverage report. Use the "uses:" directive to reference the action. See action's documentation for usage details. The artifct will be visible in the GitHub UI under the summary of the workflow run.

### 5. Test the pipeline
Commit the changes and push them to the repository. The workflow should run automatically. Check the workflow run in the GitHub UI to see if the tests pass and the code coverage report is stored.

## Complete the docker_build_and_publish job

The job should build a Docker image and publish it to ghcr.io. Add the following steps and other missing pieces to the job.

### 1. Run only after the run_tests job
Use the `needs:` directive in the job definition to make sure the *docker_build_and_publish* job runs only after the *run_tests* job.

### 2. Add write permission to GitHub Container Registry
To be able to publish the Docker image to ghcr.io, you need to give the worlflow permissions to write to GitHub Packages. Add `packages: write` to the permissions section of the job definition.

### 3. Log in to ghcr.io
In order to push the Docker image to ghcr.io, you need to log in to the registry. Use the actions/login-action and [this example](https://github.com/docker/login-action?tab=readme-ov-file#github-container-registry) to log in to ghcr.io.

### 4. Extract metadata (tags, labels) for Docker
Use docker/metadata-action to extract metadata from the repository. The action will extract the tags and labels from the repository and store them in the environment variables that can be used in the following step when building the Docker image. See the 'Docker meta' step in the [basic example](https://github.com/docker/metadata-action?tab=readme-ov-file#basic). Use this in the "images" property of the step:

```
images: ghcr.io/${{ github.repository_owner }}/${{ github.event.repository.name }}
```

### 5. Build and push the Docker image
Finally build and push the Docker image to ghcr.io. Use the [docker/build-push-action]() to build and push the image. Use the following configuration for the action:

```
   push: true
   tags: ${{ steps.meta.outputs.tags }}
   labels: ${{ steps.meta.outputs.labels }}
```

The valus of tags and labels are extracted by the metadata-action in the previous step.

### 6. Test the pipeline
Commit the changes and push them to the repository. The workflow should run automatically. Check the workflow run in the GitHub UI to see if the tests pass and the Docker image is published to ghcr.io.