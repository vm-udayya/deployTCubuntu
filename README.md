# Deployment Guide for Ubuntu 22.04 using TeamCity

This guide provides step-by-step instructions to deploy a build on Ubuntu 22.04 using TeamCity. It covers the installation of the TeamCity agent, creation of build configurations, and execution of deployment commands via SSH.

## Key Steps

### 1. Install TeamCity Agent on Ubuntu

- **Download the TeamCity Agent Package:**
  Download the latest TeamCity agent package for Linux from the TeamCity website.

- **Install the Agent:**
  Use the appropriate package manager to install the agent. For example, using `dpkg`:
  ```bash
  sudo dpkg -i teamcity-agent-linux.zip
  ```

- **Configure the Agent:**
  Edit the `buildAgent.properties` file located in the `conf` directory of the agent installation. Provide the TeamCity server URL and authentication details:
  ```properties
  serverUrl=http://<your-teamcity-server>:8111
  name=<agent-name>
  ```

### 2. Create a Build Configuration in TeamCity

- **Project Setup:**
  Create a new project in TeamCity to manage your build configuration.

- **Build Steps:**
  - **Artifact Download:**
    Add a build step to download built artifacts from a previous build stage if necessary.
  
  - **SSH Execution:**
    Add an "SSH Exec" build step to connect to your Ubuntu server and execute the deployment script. Specify the following:
    - SSH Host
    - Username
    - Private Key

    Example command to execute:
    ```bash
    ssh -i /path/to/private/key user@hostname 'bash -s' < /path/to/deployment/script.sh
    ```

### 3. Configure Triggers

- **VCS Trigger:**
  Set up a trigger to automatically initiate the build when changes are detected in your version control system.

- **Schedule Trigger:**
  Optionally, configure a scheduled build to deploy at specific intervals.

## Important Considerations

### Permissions
Ensure the TeamCity agent user on Ubuntu has sufficient permissions to execute the deployment commands on the server.

### Security
- Use SSH keys for secure authentication when connecting to the Ubuntu server.
- Ensure that your private key is stored securely and has the correct permissions:
  ```bash
  chmod 600 /path/to/private/key
  ```

### Deployment Script
Write a robust deployment script that:
- Handles potential errors.
- Ensures a smooth deployment process.
- Copies artifacts to the appropriate location, restarts services, or performs other necessary actions.

## Conclusion
Following these steps will help you set up a TeamCity agent on Ubuntu 22.04 and create a build configuration for deploying your applications effectively. For further details, refer to the TeamCity documentation.
