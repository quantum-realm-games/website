# Deploying to Bluehost using Git and SSH

This guide outlines the steps to deploy your static website from a Git repository (like GitHub) to your Bluehost hosting account using SSH and Git.

**Last Updated:** May 30, 2025

## Prerequisites

1.  **Bluehost Account:** You have an active Bluehost hosting account.
2.  **SSH Access:** SSH access must be enabled for your Bluehost account. You can usually enable this through your Bluehost cPanel or by contacting Bluehost support.
3.  **Git Installed Locally:** You have Git installed on your local machine and your project is a Git repository.
4.  **Project Pushed to Remote Repository:** Your website code is pushed to a remote Git repository (e.g., GitHub, GitLab, Bitbucket).
5.  **SSH Client:** You have an SSH client installed on your local machine (e.g., Terminal on macOS/Linux, PowerShell or PuTTY on Windows).

## Method 1: Manual Git Pull via SSH

This method involves connecting to your server via SSH and manually pulling the latest changes from your Git repository.

### Step 1: Connect to Bluehost via SSH

1.  Open your SSH client (Terminal or PowerShell).
2.  Use the following command to connect, replacing `your_bluehost_username` with your cPanel username and `yourdomain.com` with your actual domain name or the server IP address provided by Bluehost:
    ```powershell
    ssh your_bluehost_username@yourdomain.com
    ```
3.  You will be prompted for your cPanel password.

### Step 2: Navigate to Your Web Root Directory

1.  Once connected, navigate to the web root directory for your domain. This is typically `public_html` for your primary domain.
    ```bash
    cd public_html
    ```
    If you are deploying to an addon domain or subdomain, the path might be different (e.g., `public_html/youraddondomain.com`).

### Step 3: Initial Setup - Clone Your Repository (First Time Only)

If this is the first time you are deploying this site to this location using Git:

1.  **Important:** Back up or move any existing files in `public_html` if it's not empty and you don't want to overwrite them.
    ```bash
    # Example: Create a backup directory and move files
    # mkdir ../public_html_backup
    # mv * .[^.]* ../public_html_backup/
    ```
2.  Clone your Git repository into the current directory (`public_html`). Replace `your_repository_url` with the HTTPS or SSH URL of your Git repository.
    ```bash
    git clone your_repository_url .
    ```
    The `.` at the end ensures the repository contents are cloned directly into the current folder, not into a new subfolder.

### Step 4: Deploying Updates

Once the repository is cloned on the server:

1.  Ensure your latest local changes are committed and pushed to your remote Git repository (e.g., GitHub).
2.  Connect to your Bluehost server via SSH (Step 1).
3.  Navigate to your web root directory (Step 2).
4.  Pull the latest changes from your repository. If your main branch is `main` or `master`:
    ```bash
    git pull origin main
    ```
    Or, for a specific branch:
    ```bash
    git pull origin your_branch_name
    ```
5.  Your website files on Bluehost will now be updated with the latest version from your repository.

## Method 2: Using cPanel's Git Version Control Feature

Bluehost cPanel often includes a "Git Version Control" feature that can help manage and deploy repositories. This can be a more user-friendly way to set up the initial clone and manage deployments.

Refer to the official cPanel documentation for detailed instructions: [cPanel Git Version Control Documentation](https://docs.cpanel.net/cpanel/files/git-version-control/)

**General Workflow with cPanel Git Version Control:**

1.  **Access cPanel:** Log in to your Bluehost account and navigate to cPanel.
2.  **Open Git Version Control:** Find and open the "Git Version Control" tool (usually under the "Files" section).
3.  **Create or Clone Repository:**
    *   You can use this interface to clone an existing remote repository (like your GitHub repo) directly to a specified directory on your server (e.g., `public_html`).
    *   You can also create a new repository on the server if needed.
4.  **Manage Repository:** Once a repository is set up through the cPanel interface, you can:
    *   View its history.
    *   Switch branches.
    *   **Pull updates** from the remote repository.
5.  **Deployment:** The cPanel interface usually provides a "Deploy" or "Update from Remote" option for managed repositories. This essentially performs a `git pull` and can also handle post-deployment actions if configured.

**Key advantages of using the cPanel tool:**
*   User-friendly interface, less command-line work for basic operations.
*   Can help manage deployment paths and SSH keys for private repositories more easily.
*   May offer features like automatic deployment on push (webhooks), though this depends on the specific cPanel version and Bluehost's configuration.

## Important Considerations

*   **`.htaccess` File:** If your WordPress site had a custom `.htaccess` file, ensure it's either removed or replaced with one suitable for a static site if necessary. For most basic static sites, you might not need one in `public_html` unless you have specific redirect or caching rules.
*   **File Permissions:** Ensure your files have the correct permissions to be served by the webserver. Typically, folders should be `755` and files `644`.
*   **SSH Keys for Private Repositories:** If your Git repository is private, you'll need to set up SSH keys. The cPanel Git Version Control tool often has a section to manage SSH keys for this purpose. If deploying manually via SSH, you would add the server's public SSH key to your Git provider (e.g., as a deploy key on GitHub).
*   **Submodules:** If your project uses Git submodules, ensure your deployment process correctly initializes and updates them (`git submodule update --init --recursive`).
*   **Build Steps:** If your static site requires a build step (e.g., for SASS, JavaScript minification), you should perform this build step *before* pushing to your Git repository, or set up a more advanced CI/CD pipeline (like GitHub Actions) to handle the build and then deploy the built files.

## Troubleshooting

*   **"Permission denied" errors:** Check SSH key setup (for private repos) or file/folder permissions on the server.
*   **Old site still showing:** Clear your browser cache and any server-side caching (if enabled on Bluehost, e.g., via Endurance Page Cache).
*   **`git` command not found:** Contact Bluehost support to confirm if Git is available via SSH and how to access it.

This guide provides a starting point. Always refer to the latest Bluehost and cPanel documentation for the most up-to-date information.
