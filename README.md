# SonarCloudScan-DAST

- name: SonarCloud Scan
  uses: SonarSource/sonarcloud-github-action@v1.4

Scan your code with SonarCloud

Using this GitHub Action, scan your code with SonarCloud to detects bugs, vulnerabilities and code smells in more than 20 programming languages!

SonarCloud is the leading product for Continuous Code Quality & Code Security online, totally free for open-source projects. It supports all major programming languages, including Java, JavaScript, TypeScript, C#, C/C++ and many more. If your code is closed source, SonarCloud also offers a paid plan to run private analyses.
Requirements

    Have an account on SonarCloud. Sign up for free now if it's not already the case!
    The repository to analyze is set up on SonarCloud. Set it up in just one click.

Usage

Project metadata, including the location to the sources to be analyzed, must be declared in the file sonar-project.properties in the base directory:

sonar.organization=<replace with your SonarCloud organization key>
sonar.projectKey=<replace with the key generated when setting up the project on SonarCloud>

# relative paths to source directories. More details and properties are described
# in https://sonarcloud.io/documentation/project-administration/narrowing-the-focus/ 
sonar.sources=.

The workflow, usually declared in .github/workflows/build.yml, looks like:

on:
  # Trigger analysis when pushing in master or pull requests, and when creating
  # a pull request. 
  push:
    branches:
      - master
  pull_request:
      types: [opened, synchronize, reopened]
name: Main Workflow
jobs:
  sonarcloud:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        # Disabling shallow clone is recommended for improving relevancy of reporting
        fetch-depth: 0
    - name: SonarCloud Scan
      uses: sonarsource/sonarcloud-github-action@master
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

You can change the analysis base directory by using the optional input projectBaseDir like this:

uses: sonarsource/sonarcloud-github-action@master
with:
  projectBaseDir: my-custom-directory

In case you need to add additional analysis parameters, you can use the args option:

- name: Analyze with SonarCloud
  uses: sonarsource/sonarcloud-github-action@master
  with:
    projectBaseDir: my-custom-directory
    args: >
      -Dsonar.organization=my-organization
      -Dsonar.projectKey=my-projectkey
      -Dsonar.python.coverage.reportPaths=coverage.xml
      -Dsonar.sources=lib/
      -Dsonar.test.exclusions=tests/**
      -Dsonar.tests=tests/
      -Dsonar.verbose=true

More information about possible analysis parameters is found in the documentation at: https://sonarcloud.io/documentation/analysis/analysis-parameters/
Secrets

    SONAR_TOKEN – Required this is the token used to authenticate access to SonarCloud. You can generate a token on your Security page in SonarCloud. You can set the SONAR_TOKEN environment variable in the "Secrets" settings page of your repository.
    GITHUB_TOKEN – Provided by Github (see Authenticating with the GITHUB_TOKEN).
