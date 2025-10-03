# DevSecOps pipeline project: 
# YouTube App CI/CD Pipeline on Azure VM
-----------------------------------------------------------------------------------

## Project Description
This project implements a **CI/CD pipeline** to **build, scan, push, and deploy** a YouTube application (Node.js/React or backend service) on an **Azure virtual machine**.  
The pipeline uses the following tools:

- **Jenkins**: Manages the CI/CD pipeline to automate build, test, push Docker image, and deploy.
- **SonarQube**: Performs code quality checks and detects potential bugs.
- **Trivy**: Scans Docker images for security vulnerabilities.
- **Docker**: Builds and runs containers for the application.
- **Azure VMs**: Hosts Jenkins, SonarQube, and runs the deployed containers.

---

## Architecture Overview

![Flow Diagram](https://drive.google.com/file/d/1auhMns1aqbxw2wCQZ8GXcge0FgkvPGDc/view?usp=sharing)
-----------------------------------------------------------------------------------
