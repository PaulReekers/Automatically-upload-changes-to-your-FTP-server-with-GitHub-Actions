# Automatically upload changes to your FTP server with GitHub Actions 🚀
If you're using a shared hosting for your project or you deployment method is through FTP, you can  automate your code deploys! 

The workflow is triggered by events such as code commits or merges. 
With GitHub Actions, you set up the environment, including any necessary dependencies or secrets (like FTP credentials). 
Using an FTP action, the workflow connects to your FTP server and uploads the changed files from your repository. 
The workflow logs the outcome of the transfer, letting you know if it succeeded or if there were issues.

This automation ensures your FTP server always has the latest version of your files without manual intervention and it can easily be applied for any website, for example, on Vue JS or even plain Javascript, HTML, and CSS.

## Setup your secrets:

**Settings → Secrets and variables → Actions**

Click “New repository secret”

- Name: `FTP_SERVER`

- Secret: `your FTP server address`

Press “Add secret”

---

Click “New repository secret”

- Name: `FTP_USERNAME`

- Secret: `your FTP username`

Press “Add secret”

---

Click “New repository secret”

- Name: `FTP_PASSWORD`

- Secret: `your FTP password`

Press “Add secret”

---
## Setup new workflow:

**Actions → New workflow**

Click “set up a workflow yourself “

Paste the YAML code below

```yaml
name: 🚀 Build and deploy to FTP

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    name: 🔨 Build
    runs-on: ubuntu-latest
    steps:
      - name: 🚚 Checkout repository
        uses: actions/checkout@v2

      - name: 🛠️ Install dependencies
        run: npm ci

      - name: 🏗️ Build project
        run: npm run build

      - name: 🗿 Archive production artifact
        uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist

  deploy:
    name: 🎉 Deploy
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: 🚚 Checkout repository
        uses: actions/checkout@v2

      - name: 🗿 Download artifact
        uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist

      - name: 📂 List files in dist directory
        run: ls -la dist

      - name: 📂 Sync files to FTP server
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
