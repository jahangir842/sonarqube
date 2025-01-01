### Guide: Installing SonarQube Using Docker

SonarQube is a popular platform for code quality analysis. Installing it with Docker is straightforward. Here's a step-by-step guide.

---

#### **Prerequisites**
1. **Docker** and **Docker Compose** installed.
   - [Install Docker](https://docs.docker.com/get-docker/)
   - [Install Docker Compose](https://docs.docker.com/compose/install/)
2. At least **2GB of RAM** for SonarQube to function properly.
3. Ensure ports **9000** and **5432** are free on your system.

---

#### **Step 1: Pull SonarQube Docker Images**
SonarQube relies on two main images:
1. **SonarQube Server**: The core application.
2. **PostgreSQL**: The database for storing analysis results.

Run the following commands to pull these images:
```bash
docker pull sonarqube:latest
docker pull postgres:latest
```

---

#### **Step 2: Create a `docker-compose.yml` File**

Create a new file named `docker-compose.yml` and add the following content:
```yaml
version: "3.8"

services:
  postgres:
    image: postgres:latest
    container_name: sonarqube-db
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonarqube
    volumes:
      - sonarqube_data:/var/lib/postgresql/data
    networks:
      - sonarqube-network

  sonarqube:
    image: sonarqube:latest
    container_name: sonarqube
    depends_on:
      - postgres
    ports:
      - "9000:9000"
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://postgres:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_extensions:/opt/sonarqube/extensions
    networks:
      - sonarqube-network

volumes:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_logs:
  sonarqube_extensions:

networks:
  sonarqube-network:
```

---

#### **Step 3: Start the SonarQube Stack**

Run the following command in the directory containing the `docker-compose.yml` file:
```bash
docker-compose up -d
```
This will:
- Start the **PostgreSQL** database container.
- Start the **SonarQube** application container.

---

#### **Step 4: Access SonarQube**
1. Open your browser and navigate to: [http://localhost:9000](http://localhost:9000)
2. Log in using the default credentials:
   - **Username**: `admin`
   - **Password**: `admin`
3. Change the password when prompted.

---

#### **Step 5: Verify Installation**

To check the running containers:
```bash
docker ps
```

Expected output:
- A `sonarqube` container running on port `9000`.
- A `postgres` container for the database.

---

#### **Step 6: Stop or Remove SonarQube**

To stop the containers:
```bash
docker-compose down
```

To remove all data volumes:
```bash
docker-compose down -v
```

---

### Additional Notes

- For production, update the database credentials and configure persistent storage paths outside Docker volumes.
- Use a reverse proxy like **Nginx** or **Traefik** for secure access via HTTPS.

This guide will set up a local SonarQube instance using Docker, ready for code analysis. Let me know if you need help with further customization!
