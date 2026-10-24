# Homies - Regular Exam Task 2

## Local build and tests

Install the .NET 6 SDK selected by `global.json`, then run from the repository root:

```shell
dotnet restore Homies.sln
dotnet build Homies.sln --configuration Release --no-restore
dotnet test Homies.sln --configuration Release --no-build --no-restore --verbosity normal
```

The solution includes both `Homies.Tests` and `Homies.IntegrationTests`.

## GitHub Actions

`.github/workflows/ci.yml` restores dependencies, builds the solution, and runs all tests on pushes to `develop` and `feature/**` branches.

## Jenkins

Configure a Pipeline job with these settings:

- Definition: Pipeline script from SCM
- SCM: Git
- Repository URL: `https://github.com/PeterMonev/SEDO-09-2026-Regular-Exam-2.git`
- Branch specifier: `*/main`
- Script path: `Jenkinsfile`
- Agent requirements: Git and the .NET SDK selected by `global.json` available on PATH.

Run the job once to initialize its trigger. The Jenkinsfile then polls SCM every two minutes and runs checkout, restore, build, and all tests when `main` changes. Polling detects new commits without a publicly reachable webhook endpoint.

The pipeline uses `bat` on Windows and `sh` on Unix agents. A failed restore, build, or test command fails the pipeline.
