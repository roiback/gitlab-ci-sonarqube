# gitlab-ci-sonarqube

Docker image that combines [sonnar-scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner), [sonar-gitlab-plugin](https://github.com/gabrie-allaigre/sonar-gitlab-plugin) and [sonar-gate-breaker](https://github.com/gabrie-allaigre/sonar-gate-breaker) to integrate SonarQube quality gates with Gitlab CI and break pipelines when the Quality Gate does not pass.

Tested with Sonarqube Community Build 25.2.0.102705 and Gitlab 17.10.1.

## Configuration

### In Sonarqube

If not installed yet, install [sonar-gitlab-plugin](https://github.com/gabrie-allaigre/sonar-gitlab-plugin) in your Sonarqube server, and follow the instructions to configure it](https://github.com/gabrie-allaigre/sonar-gitlab-plugin#configuration).

### In your project

As with [sonnar-scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner), you will need to have a `sonar.properties` file in your project's root folder.

To run the scan, add the following to your `gitlab-ci.yml`

```yml
quality:
  image: roiback/gitlab-ci-sonarqube
  stage: test
  script:
    - sonar-scanner # you can add other sonar options e.g: -Dsonar.projectVersion=${CI_COMMIT_SHA}
```

And add these variables to Gitlab CI/CD:
```
SONAR_TOKEN = "..."
SONAR_HOST_URL = "https://sonarqube.example.com"
```

`sonar-scanner` publishes a scan in Sonarqube. If the scan does not pass your Sonarqube Quality Gate, the `sonar-scanner` command with exit with non-zero status, breaking your pipeline.
