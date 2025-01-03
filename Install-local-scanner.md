### **Installing SonarQube Scanner on Windows and Linux**

Here’s a step-by-step guide to installing the SonarQube scanner on both operating systems:

---

### **1. For Windows**

#### **Step 1: Download the SonarQube Scanner**
- Visit the [SonarQube Downloads page](https://docs.sonarsource.com/).
- Download the latest version of **SonarQube Scanner** for your platform.

#### **Step 2: Extract the Files**
- Extract the downloaded ZIP file to a desired location, such as `C:\sonar-scanner`.

#### **Step 3: Configure the Scanner**
1. Open the `conf/sonar-scanner.properties` file in a text editor.
2. Set the **SonarQube server URL**:
   ```properties
   sonar.host.url=http://<sonarqube-server-url>
   ```
3. (Optional) If using authentication, add:
   ```properties
   sonar.login=<your-authentication-token>
   ```

#### **Step 4: Add SonarQube Scanner to PATH**
1. Open **Control Panel > System > Advanced System Settings > Environment Variables**.
2. Under **System Variables**, find `Path` and click **Edit**.
3. Add the path to the `bin` directory of the SonarQube Scanner (e.g., `C:\sonar-scanner\bin`).
4. Save the changes.

#### **Step 5: Verify Installation**
- Open **Command Prompt** and run:
  ```bash
  sonar-scanner -v
  ```
- This should display the SonarQube Scanner version.

---

### **2. For Linux**

#### **Step 1: Download the SonarQube Scanner**
- Visit the [SonarQube Downloads page](https://docs.sonarsource.com/).
- Download the latest version of **SonarQube Scanner** for Linux.

#### **Step 2: Extract the Files**
- Extract the downloaded TAR file to a desired location:
  ```bash
  tar -xvzf sonar-scanner-cli-*.tar.gz -C /opt/
  mv /opt/sonar-scanner-<version> /opt/sonar-scanner
  ```

#### **Step 3: Configure the Scanner**
1. Open the configuration file:
   ```bash
   nano /opt/sonar-scanner/conf/sonar-scanner.properties
   ```
2. Set the **SonarQube server URL**:
   ```properties
   sonar.host.url=http://<sonarqube-server-url>
   ```
3. (Optional) If using authentication, add:
   ```properties
   sonar.login=<your-authentication-token>
   ```

#### **Step 4: Add SonarQube Scanner to PATH**
1. Open your shell configuration file (e.g., `.bashrc` or `.zshrc`):
   ```bash
   nano ~/.bashrc
   ```
2. Add the following line:
   ```bash
   export PATH=$PATH:/opt/sonar-scanner/bin
   ```
3. Save the file and reload the shell configuration:
   ```bash
   source ~/.bashrc
   ```

#### **Step 5: Verify Installation**
- Run the following command:
  ```bash
  sonar-scanner -v
  ```
- This should display the SonarQube Scanner version.

---

### **Next Steps**
1. **Create a `sonar-project.properties` file** in the root directory of your project with configurations like:
   ```properties
   sonar.projectKey=my_project_key
   sonar.projectName=My Project
   sonar.projectVersion=1.0
   sonar.sources=.
   sonar.host.url=http://<sonarqube-server-url>
   sonar.login=<authentication-token>
   ```
2. Run the scanner from your project directory:
   ```bash
   sonar-scanner
   ```

---

Let me know if you face any issues during installation or configuration!
