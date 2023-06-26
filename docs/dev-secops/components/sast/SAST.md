# Static Application Security Testing

We use the SonarQube for SAST.

```bash
sonar-scanner \
  -Dsonar.projectKey=sample-application \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=sqp_5aff647b29e5957e34c808dbc5d41812ec866346
```