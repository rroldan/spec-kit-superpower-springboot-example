# Java Formatting and Style Guide (Google)

This document explains how to integrate the Google Java Style Guide into a Java/Spring Boot project in this repository. It recommends using both an automatic formatter (`google-java-format`) and a CI-enforced Checkstyle rule set (Google checks). The recommended approach minimizes developer friction while ensuring CI enforces the canonical style.

Recommended setup (Option C):

- Use `google-java-format` (auto-formatter) for local and IDE formatting.
- Enforce `google_checks` via `maven-checkstyle-plugin` in CI with `failOnViolation=true`.

1) Maven snippets

Add these plugin snippets to your `pom.xml` (examples):

google-java-format via the Coveo formatter plugin:

```xml
<plugin>
  <groupId>com.coveo</groupId>
  <artifactId>fmt-maven-plugin</artifactId>
  <version>2.11</version>
  <configuration>
    <style>GOOGLE</style>
  </configuration>
  <executions>
    <execution>
      <goals>
        <goal>format</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

Checkstyle plugin (Google checks):

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-checkstyle-plugin</artifactId>
  <version>3.2.2</version>
  <configuration>
    <configLocation>google_checks.xml</configLocation>
    <failOnViolation>true</failOnViolation>
  </configuration>
  <executions>
    <execution>
      <phase>verify</phase>
      <goals>
        <goal>check</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

Note: `google_checks.xml` is the canonical Checkstyle configuration for Google style. You can include a local copy under `config/checkstyle/google_checks.xml` and set `configLocation` to that path, or rely on the plugin classpath resource.

2) GitHub Actions (CI)

Add a workflow step to run formatting and checkstyle checks (this example runs plugin goals directly so you don't need to modify `pom.xml`):

```yaml
name: Java Style Checks

on: [pull_request, push]

jobs:
  style:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 21
      - name: Check google-java-format
        run: mvn com.coveo:fmt-maven-plugin:2.11:check
      - name: Run Checkstyle
        run: mvn org.apache.maven.plugins:maven-checkstyle-plugin:3.2.2:check
```

3) Local developer setup

- Install the `google-java-format` plugin in your IDE (IntelliJ: Google Java Format plugin). Configure "Reformat on Save" or use the Maven plugin via `mvn com.coveo:fmt-maven-plugin:2.11:format`.
- Add a pre-commit hook to run `mvn com.coveo:fmt-maven-plugin:2.11:format` to auto-format changed Java files before commits.

4) Enforcing in CI vs auto-formatting

- Auto-formatting reduces friction; developers should format locally before committing. CI should still run Checkstyle with `failOnViolation=true` to catch any remaining style issues.

5) Adding to existing projects in this repo

- If this repository contains a Maven Java module, add the plugin snippets above to that module's `pom.xml` and enable the GitHub Actions workflow or adapt your existing CI to run the two plugin goals.

References:
- google-java-format: https://github.com/google/google-java-format
- Checkstyle Google Checks: https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml
