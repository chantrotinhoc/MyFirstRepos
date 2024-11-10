Analyze your project with GitHub CI

Create GitHub Secrets
In your GitHub repository, go to Settings > Secrets and create below new secrets:
Click on New repository secret.
In the Name field, enterSONAR_TOKEN
In the Value field, enter an existing token, or a newly generated one: Generate a token
Click on Add secret.
Click on New repository secret.
In the Name field, enterSONAR_HOST_URL
In the Value field, enter http://localhost:9000 
Click on Add secret.
Create Workflow YAML File
What option best describes your project?
Maven
Gradle
.NET
Other (for JS, TS, Go, Python, PHP, ...)
Create a sonar-project.properties file in your repository and paste the following code:
sonar.projectKey=Static-Code-Analysis
Repeat this step for all the projects in your monorepo
Create or update your .github/workflows/build.yml YAML file with the following content:
name: Build

on:
  push:
    branches:
      - main


jobs:
  build:
    name: Build and analyze
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Shallow clones should be disabled for a better relevancy of analysis
      - uses: sonarsource/sonarqube-scan-action@v3
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
      # If you wish to fail your job when the Quality Gate is red, uncomment the
      # following lines. This would typically be used to fail a deployment.
      # - uses: sonarsource/sonarqube-quality-gate-action@master
      #   timeout-minutes: 5
      #   env:
      #     SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
