### Guide: Downloading and Using Sonar Scanner from Docker Images

Sonar Scanner is a tool used for sending code analysis data to SonarQube. You can run Sonar Scanner as a Docker container, which simplifies the setup process.

---

#### **Step 1: Pull the Sonar Scanner Docker Image**

Run the following command to pull the official Sonar Scanner Docker image:
```bash
docker pull sonarsource/sonar-scanner-cli:latest
```

---

#### **Step 2: Verify the Download**

Check if the image is downloaded successfully by listing available Docker images:
```bash
docker images
```

You should see an entry for `sonarsource/sonar-scanner-cli`.

---

#### **Step 3: Run the Sonar Scanner**

You can use the Sonar Scanner Docker container to scan your project by mounting your project directory and passing the necessary environment variables. 

Here’s the command:
```bash
docker run --rm \
  -v "$(pwd):/usr/src" \
  -e SONAR_HOST_URL="http://your-sonarqube-server:9000" \
  -e SONAR_LOGIN="your-sonar-token" \
  sonarsource/sonar-scanner-cli:latest \
  -Dsonar.projectKey=your-project-key \
  -Dsonar.sources=.
```

- **`-v "$(pwd):/usr/src"`**: Mounts your current project directory into the container.
- **`SONAR_HOST_URL`**: URL of your SonarQube server.
- **`SONAR_LOGIN`**: Token or credentials for authentication.
- **`sonar.projectKey`**: Unique key identifying your project in SonarQube.
- **`sonar.sources`**: Path to the source files for analysis (default is the current directory).

---

#### **Step 4: Example Command**

Assuming:
- Your SonarQube server is at `http://localhost:9000`.
- You have a token `12345`.
- Your project key is `example-project`.

The command will look like this:
```bash
docker run --rm \
  -v "$(pwd):/usr/src" \
  -e SONAR_HOST_URL="http://localhost:9000" \
  -e SONAR_LOGIN="12345" \
  sonarsource/sonar-scanner-cli:latest \
  -Dsonar.projectKey=example-project \
  -Dsonar.sources=.
```

---

#### **Step 5: Analyze the Results**

Once the scan completes, it sends the results to the SonarQube server. Open your SonarQube server's dashboard in a browser and navigate to the project to view the analysis.

---

#### **Step 6: Optional Configurations**

You can pass additional configurations with `-D` flags, for example:
- Exclude files: `-Dsonar.exclusions=**/*.test.js`.
- Include test coverage: `-Dsonar.javascript.lcov.reportPaths=coverage/lcov.info`.

---

#### **Troubleshooting**

- **"Connection refused" error**: Ensure the `SONAR_HOST_URL` points to a running SonarQube instance.
- **Authentication issues**: Verify the token or credentials passed in `SONAR_LOGIN`.

---

This guide helps you use the Sonar Scanner Docker image for analyzing your projects. Let me know if you need further assistance!
