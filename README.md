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
docker pull sonarqube:community
docker pull postgres:15
```

---

#### **Step 2: Create a `docker-compose.yml` File**

Create a new file named `docker-compose.yml` and add the following content:
```yaml

version: "3.8"

services:
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    ports:
      - "9000:9000"
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
      SONAR_ES_BOOTSTRAP_CHECKS_DISABLE: "true" # Disable ES bootstrap checks for containers
      SONAR_SEARCH_JAVAOPTS: "-Xms512m -Xmx512m" # Adjust heap size for ES
    depends_on:
      - db
    restart: always
    volumes:
      - ./sonarqube/data:/opt/sonarqube/data
      - ./sonarqube/extensions:/opt/sonarqube/extensions
      - ./sonarqube/logs:/opt/sonarqube/logs
    ulimits:
      memlock:
        soft: -1
        hard: -1
    deploy:
      resources:
        limits:
          memory: 2g # Limit container memory usage to 2GB
        reservations:
          memory: 1g # Reserve at least 1GB for this container

  db:
    image: postgres:15
    container_name: postgres
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonarqube
    restart: always
    volumes:
      - ./postgres/data:/var/lib/postgresql/data
    deploy:
      resources:
        limits:
          memory: 1g # Limit container memory usage to 1GB
        reservations:
          memory: 512m # Reserve at least 512MB for this container

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
