# React Application CI/CD Pipeline

This repository contains a CI/CD pipeline setup for deploying a React application using Azure DevOps. The pipeline is defined in the `azure-pipelines.yml` file, which handles the build, artifact publishing, and deployment to a remote server using SSH.

## Prerequisites

Before using this pipeline, ensure you have the following prerequisites in place:

### 1. **Azure DevOps Agent Pool**
- An agent pool is required to run the pipeline. You can either use a self-hosted agent or Azure-hosted agent.
- To create a self-hosted agent, follow the [official documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops).
- After creating the agent, register it in the desired pool (e.g., `CoolPool`).

### 2. **SSH Endpoint for Remote Server**
You will need a Linux VM or server for deployment via SSH. Set up an SSH connection for Azure DevOps as follows:

1. **Ensure SSH is enabled on the server**:
   - Use password authentication or SSH keys to securely access the server.
   - Test SSH access locally using the command:
     ```bash
     ssh <username>@<server-ip>
     ```

2. **Create SSH Service Connection in Azure DevOps**:
   - Navigate to **Project Settings** > **Service connections** > **New service connection**.
   - Select **SSH** and fill in the required details:
     - **Host**: `<server-ip>` (e.g., `20.40.102.122`)
     - **Port**: `22` (default SSH port)
     - **User**: `<your-linux-user>` (e.g., `ubuntu`, `root`)
     - **Password or Private Key**: Provide your authentication details (either password or private key).
     - **Connection Name**: Name the connection (e.g., `frontend-connection`).

3. **Example Connection String**:
   - If you're using password-based authentication:
     ```yaml
     sshEndpoint: 'frontend-connection'
     ```

   - If you're using an SSH private key, ensure your private key is uploaded and secured in the Azure DevOps service connection.

### 3. **Node.js Version**
- Ensure that the Node.js version specified in the pipeline matches the version required by your React application. The current pipeline is set to `16.x`, but you can adjust it as needed:
  ```yaml
  - task: NodeTool@0
    inputs:
      versionSpec: '16.x'
  ```

### 4. **Project Structure**
- The `build` output of the React application is published as an artifact named `frontendArtifact`, which will be transferred via SSH to the remote server.
- Ensure the project directory includes the React application source code and a `package.json` file with the necessary dependencies.

## Pipeline Configuration

### Trigger Branch
The pipeline is triggered by pushes to the `master` branch. If you need to trigger the pipeline from another branch, modify the `trigger` section of the `azure-pipelines.yml` file:

```yaml
trigger:
  branches:
    include:
      - <your-branch-name>
```

### Pool Configuration
If you're using a different agent pool, change the pool name:

```yaml
pool:
  name: <YourAgentPoolName>
```

### SSH Configuration
Ensure your SSH service connection (e.g., `frontend-connection`) is configured correctly with proper authentication, using either password or private key.

## Pipeline Stages

The pipeline consists of two stages:

1. **Build Stage**: 
   - Installs the necessary Node.js version.
   - Runs `npm install` to fetch project dependencies.
   - Builds the React application using `npm run build`.
   - Publishes the build artifacts.

2. **Release/Deploy Stage**: 
   - Downloads the build artifacts.
   - Copies the build files to the remote server using `CopyFilesOverSSH`.
   - Moves the files to the web server directory (`/var/www/html/`) on the remote machine.

## Usage

1. Clone this repository.
2. Ensure your pipeline is set up with correct pool, branch, and SSH connection details.
3. Push your code to the `master` branch or the branch specified in the pipeline trigger.
4. The pipeline will build, publish, and deploy your React application automatically.

## License

This project is licensed under the MIT License.

---

This `README.md` now includes an additional section explaining the SSH service connection and provides an example of the connection string setup.
