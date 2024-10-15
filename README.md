# CI/CD Pipeline for Building and Pushing Docker Images

This repository demonstrates how to build Docker images for a React application and push them to Docker Hub using an Azure DevOps pipeline. It includes stages for Docker installation, Docker login, Docker image build, and Docker push.

## Prerequisites

Before you start using this pipeline, ensure that you meet the following prerequisites:

### 1. **Azure DevOps Setup**
   - A **self-hosted agent** must be configured in Azure DevOps.
   - You can either use a **Linux VM** or a **Windows machine** as the agent.

### 2. **For Linux Agents**
   - Ensure that Docker is properly configured on the VM where the agent is running:
     - Run the following commands to add the current user to the `docker` group:
       ```bash
       sudo groupadd docker
       sudo usermod -aG docker $USER
       ```
     - After running these commands, log out and back in for the changes to take effect.
   - Make sure Docker is installed and can be run without `sudo` permissions by verifying with:
     ```bash
     docker --version
     ```

### 3. **For Windows Agents**
   - If you are using a **Windows** machine as your agent, install **Docker for Windows**:
     - Download and install **Docker Desktop** from [Docker's official site](https://www.docker.com/products/docker-desktop).
     - Ensure Docker is running and added to the system `PATH`.
     - After installation, verify Docker with:
       ```bash
       docker --version
       ```

## Pipeline Overview

The pipeline includes the following stages:

1. **Docker Installation**: Installs Docker (if not already installed) using the `DockerInstaller` task.
2. **Docker Login**: Logs into Docker Hub using the provided container registry connection.
3. **Docker Build**: Builds a Docker image from the provided `Dockerfile` and tags it.
4. **Docker Push**: Pushes the built Docker image to Docker Hub with the specified tags.

### Key Notes:
- Ensure that you have created a **service connection** in Azure DevOps for Docker Hub:
  - Go to **Project Settings > Service Connections**.
  - Add a new Docker Registry service connection with your Docker Hub credentials (username and password or access token).
  
- The pipeline triggers on changes to the `react` branch but excludes the `master` and `interview` branches by default. **You can modify the trigger settings to suit your branch requirements**.
  - Example:
    ```yaml
    trigger:
      branches:
        include:
          - your-branch-name
    ```

- By default, the pipeline is set to run on a **Linux pool**. **You can change the pool setting based on the operating system of your agent**.
  - Example:
    ```yaml
    pool: your-pool-name
    ```

## Steps to Use

### 1. **Clone the Repository**
   Clone this repository to your local machine to modify or add new stages as per your requirements.

### 2. **Create a Self-Hosted Agent**
   Follow Azure DevOps documentation to create a self-hosted agent on your VM or local machine. Make sure Docker is installed and properly configured as mentioned in the prerequisites.

### 3. **Create a Docker Hub Connection**
   - In Azure DevOps, create a Docker Hub connection to enable the pipeline to log in and push images.
   - This will be used in the pipeline to authenticate and push the built Docker images.

### 4. **Customize the Pipeline**
   - Modify the pipeline YAML file to specify your Docker Hub repository, tags, and any custom build options in the `Dockerfile`.
   - **Change the trigger branches and pool details** as per your project requirements.

### 5. **Run the Pipeline**
   Once all configurations are complete, you can trigger the pipeline either by pushing changes to the specified branch or manually from the Azure DevOps interface.

## Conclusion

This pipeline is designed to automate the process of building Docker images and pushing them to Docker Hub, streamlining your development workflow. Make sure Docker is properly installed and configured on your agent machine (whether Linux or Windows) and that a Docker Hub connection string is set up in Azure DevOps before running the pipeline.

For any customization or additional stages, feel free to edit the pipeline file as per your requirements.
