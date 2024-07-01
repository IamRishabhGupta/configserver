

# Spring Cloud Config Server

This project serves as the Spring Cloud Config Server, providing centralized external configuration management for distributed microservices.

## **Table of Contents**
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Running the Server](#running-the-server)
- [Endpoints](#endpoints)
- [Security](#security)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## **Overview**
The Spring Cloud Config Server fetches configuration properties for microservices from a central Git repository. It supports multiple environments and profiles, enabling a standardized configuration management process.

## **Getting Started**

### **Prerequisites**
- Java 8 or higher
- Maven or Gradle
- A Git repository containing the configuration files (e.g., `application-dev.properties`, `application-prod.properties`)

### **Dependencies**
Ensure you have the necessary dependencies in your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### **Spring Cloud Version**
Specify the Spring Cloud version in your `pom.xml`:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud-version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## **Configuration**

### **`application.properties`**
Set up the Config Server to read from your Git repository:

```properties
server.port=8888
spring.application.name=configserver
spring.cloud.config.server.git.uri=https://github.com/yourusername/application.config
spring.cloud.config.server.git.clone-on-start=true
management.endpoints.web.exposure.include=*
```

### **Bootstrap Configuration**
You may also need `bootstrap.properties` for initializing properties before the application starts:

```properties
spring.cloud.config.server.git.searchPaths=configuration
spring.cloud.config.server.git.uri=https://github.com/yourusername/application.config
spring.cloud.config.server.git.default-label=main
```

### **Profile Configuration**
For different environments, set profiles in the request URL or in microservices' `application.properties`:

```properties
spring.profiles.active=dev
```

### **Example Structure of Config Repo**
```
/application.config
    ├── application-dev.properties
    ├── application-prod.properties
    ├── application-default.properties
```

## **Running the Server**

### **Using Maven**
Run the server using Maven:

```bash
mvn spring-boot:run
```

### **Using Java**
Alternatively, run the server as a JAR:

```bash
java -jar configserver-application.jar
```

## **Endpoints**

- **Configuration Retrieval**: Microservices retrieve their configurations via the following pattern:
  ```
  http://localhost:8888/{application}/{profile}/{label}
  ```

- **Health Check**: Check the server's health:
  ```
  http://localhost:8888/actuator/health
  ```

- **Environment**: Fetch configuration environment:
  ```
  http://localhost:8888/{application}/{profile}
  ```

### **Examples**
- Fetch the `jobms` app's development profile:
  ```
  http://localhost:8888/jobms/dev
  ```

## **Security**

### **Encryption**
Use encryption for sensitive properties. Spring Cloud Config provides support for encryption/decryption of property values.

### **Access Control**
Limit access to the Config Server to trusted IPs or secure it using basic authentication.

## **Troubleshooting**

- **Config Not Loaded**: Ensure the Git repository URL is correct and accessible.
- **Profile Not Found**: Verify that the specified profile exists in the configuration repository.
- **Network Issues**: Check network connectivity and firewall rules.

### **Common Errors**
- **404 Not Found**: Configuration file or profile does not exist.
- **Connection Refused**: Server might not be running or network issues.

## **Contributing**
If you wish to contribute:
1. Fork the repository.
2. Create a new branch.
3. Commit your changes.
4. Open a pull request.

## **License**
This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

