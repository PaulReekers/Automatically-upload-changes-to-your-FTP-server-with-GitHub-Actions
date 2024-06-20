# Automatically upload changes to your FTP server with GitHub Actions 🚀
If you're using a shared hosting for your project or you deployment method is through FTP, you can  automate your code deploys! 

If you're using a shared hosting for your project or you deployment method is through FTP, you can  automate your code deploys! The workflow is triggered by events such as code commits or merges. With GitHub Actions, you set up the environment, including any necessary dependencies or secrets (like FTP credentials). Using an FTP action, the workflow connects to your FTP server and uploads the changed files from your repository. The workflow logs the outcome of the transfer, letting you know if it succeeded or if there were issues.

This automation ensures your FTP server always has the latest version of your files without manual intervention and it can easily be applied for any website, for example, on Vue JS or even plain Javascript, HTML, and CSS.

## Setup your secrets:

**Settings → Secrets and variables → Actions**

Click “New repository secret”

- Name: `FTP_SERVER`

- Secret: `your ftp.server`

Press “Add secret”


Click “New repository secret”

- Name: `FTP_USERNAME`

- Secret: `your username`

Press “Add secret”


Click “New repository secret”

- Name: `FTP_PASSWORD`

- Secret: `your ftp password`

Press “Add secret”


## Setup new workflow:

**Actions → New workflow**

Click “set up a workflow yourself “

Paste the YAML code below

```yaml
name: 🚀 Build and deploy

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  build:
    
    name: Build 🔨
    runs-on: ubuntu-latest
    steps:
      - name: checkout repository
        uses: actions/checkout@main
      - name: Installing project dependencies
        run: npm ci
      - name: Build Dependencies
        run: npm run build
      - name: Archive production artifact
        uses: actions/upload-artifact@main 
        with:
          name: dist
          path: dist

  deploy: 
    name: Deploy 🚚 
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Get latest code
        uses: actions/checkout@main
      - name: Download artifact
        uses: actions/download-artifact@main
        with:
          name: dist
          path: dist
      - name: Sync files to host
        uses: SamKirkland/FTP-Deploy-Action@v4.3.5
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: dist/
          server-dir: httpdocs/
```
Click “Commit changes”

This will trigger the workflow, Github Actions will check the .github/workflows/ directory for YAML files that define workflows. It will look for a workflow configuration that matches the event that triggered the action.

The workflow consists of jobs, which contains steps. Each step involves running commands, setting up environments, or executing scripts.

You can monitor the progress of your workflow in the Actions tab of your GitHub repository. You’ll see logs and results of each step in real-time.

That's it! 🤩
