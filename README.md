# GitHub Actions - Parallel Jobs with Dependency Sharing

Often times, especially in a multi-tenant architecture, it's expected to have one repository from where multiple builds are generated for different clients / architectures etc. Running separate actions for each of these different clients can be expensive and tricky if we have to download dependencies for every build again and again.

In this repo, I have solved this problem using two different approaches. Although the example uses a node project, the idea can be applied to all sorts of projects.

## Approach 1 - Using Upload Artifacts Action

In this approach, we use the https://github.com/actions/upload-artifact action to compress the `node_modules` directory after the dependencies are installed and upload it to artifact repository on GitHub. In the subsequent jobs, we pull in the same artifact, extract it and use it instead of reinstalling them for every build of the project.

Check [build-using-artifacts.yml](./.github/workflows/build-using-artifacts.yml).

## Approach 2 - Using Cache Action

In this approach, we use the https://github.com/actions/cache action to cache the `node_modules` directory across jobs. We cache the directory against the has of the package-lock.json file and as long as the lock file hasn't changed, the same cache of `node_modules` is used.
This approach is at least as optimized as the artifacts approach since package-lock.json file doesn't always change with every run of the workflow in the real world.

Check [build-using-cache.yml](./.github/workflows/build-using-artifacts.yml).
