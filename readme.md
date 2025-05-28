# Jenkins Local Setup Guide

This guide provides step-by-step instructions to install and configure Jenkins locally.

---

## Prerequisites

- Java (JDK 11 or higher)
- A modern web browser
- Admin/root privileges on your machine

---

## Step 1: Install Java

Jenkins requires Java to run. You can check if Java is installed by running:

```bash
java -version
```

### If Java is not installed, install it:

```bash
sudo apt update
sudo apt install openjdk-11-jdk
```

---

## Step 2: Download and Run Jenkins

Download the Jenkins `.war` file from the [official site](https://www.jenkins.io/download/).

Navigate to the folder where the file is downloaded and run:

```bash
java -jar jenkins.war
```

This starts Jenkins on the default port **8080**. You can access it at: [http://localhost:8080](http://localhost:8080)

---

## Step 3: Unlock Jenkins

1. When Jenkins starts for the first time, it displays a setup screen asking for an unlock key.
2. Open the file mentioned in the terminal output to retrieve the password. Usually located at:

```bash
~/.jenkins/secrets/initialAdminPassword
```

3. Copy the password and paste it into the setup screen in your browser.

---

## Step 4: Install Suggested Plugins

Click **Install suggested plugins** to let Jenkins install the most commonly used plugins automatically.

---

## Step 5: Create First Admin User

- Fill in the required fields to create the first admin account.
- Click **Save and Continue**.

---

## Step 6: Run Jenkins as a System Service (Optional)

To run Jenkins as a system service on Ubuntu/Debian:

```bash
sudo apt install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

---

## Step 7: GitHub Setup

1. Create a new Jenkins job and name it `Notes-app`.
2. Add a meaningful description for the project.
3. In the **Source Code Management** section, select **Git** and provide the GitHub repository URL.
4. Enable the **GitHub Project** option and enter the project URL.
5. Under **Build Triggers**, choose based on your environment:
   - If running on AWS or GCP: select **GitHub hook trigger for GITScm polling**.
   - If running locally: select **Poll SCM** and specify a cron schedule. Example:

```cron
H/5 * * * *
```

---

## Step 8: Define the Pipeline

Choose how to define your Jenkins pipeline:

### Option 1: Pipeline Script

Select this if you want to define the pipeline directly in the Jenkins UI.

### Option 2: Pipeline Script from SCM

Select this if your GitHub repository contains a `Jenkinsfile`. Jenkins will automatically use this file to configure and run your pipeline.

---

## Step 9: Configure Credentials

1. Go to **Manage Jenkins** → **Credentials** → **(Global)**.
2. Click on **Add Credentials**.
3. Choose the credential type (e.g., **Username with password**, **SSH key**, or **Secret text**).
4. Enter the required information and click **Create**.
5. Reference the credential ID in your Jenkins pipeline script where needed.

---

## Useful Links

- [Jenkins Official Website](https://www.jenkins.io/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Pipeline Syntax Generator](https://www.jenkins.io/doc/book/pipeline/syntax/)