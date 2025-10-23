+++
date = '2025-10-15T13:10:14+09:00'
draft = false
title = 'Installation Guide'
+++

# NicolNavi Installation Guide

This guide walks you through getting NicolNavi up and running. You can either rely on Docker for a managed setup or run the app directly in a Python environment. Review the checklist below before you start.

## Before You Begin

- Supported systems: Windows 10 (64-bit) or later, macOS Monterey (12) or later, and popular Linux distributions
- Permissions: Administrator rights may be required during the initial setup
- Browser: Latest Google Chrome (recommended)
- Memory: 16 GB RAM or more is strongly recommended; lower-spec systems may not run reliably

{{< notice info >}}Other browsers might work, but only Google Chrome is officially verified.{{< /notice >}}

## Picking an Approach

- **Use the Docker setup** when you want a standardized environment with minimal manual configuration.
- **Use the Python workflow** if you prefer to manage dependencies yourself or plan to extend the codebase.

Choose one path and follow it through to the end.

---

## Installing with Docker

### Step 1. Prepare Required Software

1. **Install Google Chrome (if needed)**  
   Download the latest installer from https://www.google.com/chrome/ and follow the prompts.
2. **Install Docker Desktop**  
   Visit https://www.docker.com/ja-jp/get-started/, download `Docker Desktop Installer.exe` for Windows or `Docker.dmg` for macOS, and complete the installer. When you see “Installation succeeded,” select “Close and log out,” then reboot your computer. On restart, Docker Desktop launches automatically—accept the license agreement when prompted. Signing in is optional.

{{< notice info >}}Windows users may be asked to enable or update WSL2. Follow the on-screen instructions, reboot if requested, and open Docker Desktop again.{{< /notice >}}

### Step 2. Download NicolNavi

1. Open the repository at https://github.com/Tan-Furukawa/nicolnavi.
2. Click `Download ZIP`, save the archive, and extract it to a convenient location.

![Download from GitHub](/images/page/install/install_github.png)

### Step 3. Start Docker Desktop

1. Launch Docker Desktop manually so it is running in the background.

{{< notice warning >}}If Docker Desktop is stopped, the setup scripts cannot prepare the environment. Confirm that the whale icon appears in the task tray (Windows) or menu bar (macOS) and that the status reads `Docker Desktop is running`.{{< /notice >}}

### Step 4. Run the Installer Script

1. In the extracted NicolNavi folder, double-click the script that matches your operating system:
   - Windows: `win_nicolnavi_install.bat`
   - macOS / Linux: `mac_nicolnavi_install.sh`
2. A terminal window opens and downloads everything the app needs. When the window reports completion, you can close it.

{{< notice info >}}If macOS or Linux shows a permission warning, open a terminal in the same folder, run `chmod +x mac_nicolnavi_install.sh`, and then launch the script again.{{< /notice >}}

### Step 5. Verify the Launch

1. From the same folder, start the application with the appropriate script:
   - Windows: `win_nicolnavi_exe.bat`
   - macOS / Linux: `mac_nicolnavi_exe.sh`
2. Open Google Chrome and visit `http://localhost:8551/`. If you see the NicolNavi interface, the setup is complete.

{{< notice info >}}You must run the launch script before visiting `http://localhost:8551/`; otherwise the page will not load.{{< /notice >}}

{{< notice info >}}NicolNavi runs locally inside the Docker environment, so you can keep using it even when offline.{{< /notice >}}

---

## Installing with a Python Environment

### Step 1. Confirm Prerequisites

1. **Google Chrome**  
   Install it from the link above if you have not already.
2. **Python and pipenv**  
   Ensure Python 3.10 or newer is installed and that the `pip` command works. Update pip with `pip install --upgrade pip`, then install pipenv using `pip install pipenv`.

### Step 2. Download NicolNavi

1. Grab the ZIP archive from https://github.com/Tan-Furukawa/nicolnavi and extract it.
2. Open a terminal and move into the project directory.

```bash
cd /path/to/nicolnavi
```

### Step 3. Set Up the Virtual Environment

1. Run the following commands to create and populate a pipenv environment:

```bash
pipenv shell
pipenv install
```

2. Wait for `Pipfile.lock` to be generated and dependency installation to finish.

{{< notice info >}}The prompt should show the virtual environment name—typically something like `(nicolnavi)`. Type `exit` to leave the environment when you are done.{{< /notice >}}

### Step 4. Launch the Application

1. With the pipenv shell still active, start the UI:

```bash
python gui/src/main.py
```

2. Open Google Chrome and go to `http://localhost:8551/` to confirm that NicolNavi loads.

---

## Troubleshooting

- **Nothing loads in the browser**: Make sure the launch script or `python gui/src/main.py` is still running. Restart it if necessary.
- **Port 8551 already in use**: Another application is occupying the port. Stop that application or adjust NicolNavi’s port setting.
- **Docker downloads stall**: Check your internet connection. In corporate networks, confirm that Docker Desktop is configured with your proxy settings.
- **`pipenv install` fails**: Run `pipenv --rm` to remove the environment and repeat Step 3.

If you encounter issues you cannot resolve, note any error messages and contact the development team for assistance.
