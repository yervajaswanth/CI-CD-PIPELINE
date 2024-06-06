# CI/CD Pipeline for BoardGame Application

This repository contains the CI/CD pipeline configuration for the BoardGame application. The pipeline is defined using Jenkins declarative syntax and incorporates various stages for building, testing, scanning, and deploying the application.

## Pipeline Overview

The pipeline consists of the following stages:

1. **Git Checkout**: Clones the repository from the specified Git URL.
   
   This stage checks out the source code from the main branch of the repository using Git.

2. **Compile**: Compiles the application using Maven.
   
   This stage compiles the Java source code using Maven build tool.

3. **Test**: Executes the unit tests for the application.
   
   This stage runs the unit tests to ensure the correctness of the application's functionality.

4. **File System Scan**: Scans the file system for vulnerabilities using Trivy.
   
   Trivy is a vulnerability scanner for containers and other artifacts. This stage scans the file system for vulnerabilities.

5. **SonarQube Analysis**: Performs static code analysis using SonarQube.
   
   SonarQube is used for static code analysis to identify code quality issues, security vulnerabilities, and bugs.

6. **Quality Gate**: Verifies the quality gate status for the code.
   
   This stage checks if the code meets the quality gate conditions defined in SonarQube.

7. **Build**: Packages the application using Maven.
   
   This stage packages the application into a distributable format, such as a JAR file, using Maven.

8. **Publish to Nexus**: Deploys the application artifacts to Nexus repository.
   
   Nexus repository manager is used to store and manage application artifacts. This stage deploys the artifacts to Nexus.

9. **Build and Tag Docker Image**: Builds and tags the Docker image for the application.
   
   This stage builds a Docker image for the application and tags it with appropriate version information.

10. **Docker Scan**: Scans the Docker image for vulnerabilities using Trivy.
   
    This stage scans the Docker image for vulnerabilities using Trivy.

11. **Push Docker Image**: Pushes the Docker image to the Docker registry.
    
    This stage pushes the Docker image to a Docker registry for distribution and deployment.

12. **Deploy to Kubernetes**: Deploys the application to Kubernetes cluster.
    
    This stage deploys the application to a Kubernetes cluster using Kubernetes manifest files.

13. **Verifying Deployment**: Verifies the deployment status by checking pods and services.
    
    This stage checks the status of pods and services in the Kubernetes cluster to ensure successful deployment.

## Prerequisites

Before running the pipeline, ensure the following prerequisites are met:

- Jenkins instance with necessary plugins installed (e.g., Docker Pipeline, SonarQube Scanner, Kubernetes CLI).
- Maven and JDK installed on the Jenkins agent.
- Docker installed on the Jenkins agent.
- SonarQube server configured with appropriate projects and quality profiles.
- Nexus repository configured to store artifacts.
- Trivy installed for vulnerability scanning.

## Usage

1. Configure Jenkins with the necessary credentials for Git, SonarQube, Docker registry, and Kubernetes cluster.
2. Create a Jenkins pipeline job and specify this repository URL as the pipeline script source.
3. Ensure that the Jenkins agent has access to the necessary tools and environments specified in the pipeline.
4. Trigger the pipeline manually or set up webhooks for automatic triggering on code changes.


