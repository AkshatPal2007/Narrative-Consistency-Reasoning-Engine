# 🚀 Installation Guide for Absolute Beginners

Welcome! This guide will walk you through installing Python on Windows Subsystem for Linux (WSL) and running the KDSH project step by step.

---

## Table of Contents
1. [What You Need](#what-you-need)
2. [Step 1: Install WSL](#step-1-install-wsl)
3. [Step 2: Install Python](#step-2-install-python)
4. [Step 3: Set Up the Project](#step-3-set-up-the-project)
5. [Step 4: Run the Project](#step-4-run-the-project)
6. [Troubleshooting](#troubleshooting)

---

## What You Need

Before starting, make sure you have:
- **Windows 10/11** (Pro, Enterprise, or Home edition)
- **Administrator access** on your computer
- **Internet connection**
- **About 5-10 GB of free disk space** (for Python, dependencies, and models)

---

## Step 1: Install WSL

### 1.1 Open PowerShell as Administrator

1. Press **Windows Key + R**
2. Type: `powershell`
3. Right-click on PowerShell and select **"Run as administrator"**
4. Click **"Yes"** when asked for permission

### 1.2 Install WSL2

In the PowerShell window, copy and paste this command:

```powershell
wsl --install
```

**What this does:** Installs WSL2 and Ubuntu automatically.

### 1.3 Restart Your Computer

After the command finishes, **restart your computer**.

### 1.4 Set Up Ubuntu Username and Password

1. After restart, Ubuntu will open automatically
2. **Create a username** (e.g., `kdsh_user`) - use lowercase, no spaces
3. **Create a password** (you won't see characters as you type - that's normal)
4. **Confirm the password** by typing it again

Your Ubuntu terminal is now ready! 🎉

---

## Step 2: Install Python

### 2.1 Update Package Manager

In your Ubuntu terminal, paste this command:

```bash
sudo apt update && sudo apt upgrade -y
```

**What this does:** Updates Ubuntu's package manager to get the latest software lists.

### 2.2 Install Python 3.12

Paste this command:

```bash
sudo apt install python3.12 python3.12-venv python3.12-dev -y
```

**What this does:** Installs Python 3.12 (the version needed for this project).

### 2.3 Verify Python Installation

Paste this command to check Python was installed correctly:

```bash
python3.12 --version
```

You should see: `Python 3.12.x` (where x is a version number)

✅ **Great!** Python is installed!

---

## Step 3: Set Up the Project

### 3.1 Navigate to the Project Folder

Paste this command in Ubuntu:

```bash
cd /mnt/c/Users/YourUsername/Desktop/Hackathon/KDSH
```

**Replace `YourUsername`** with your actual Windows username!

💡 **Tip:** You can find your username by looking at the folder path in Windows File Explorer.

### 3.2 Create a Virtual Environment

A virtual environment is like a separate Python installation just for this project. Paste:

```bash
python3.12 -m venv venv
```

**What this does:** Creates a folder called `venv` with isolated Python and packages.

### 3.3 Activate the Virtual Environment

This "turns on" your isolated Python. Paste:

```bash
source venv/bin/activate
```

**Success indicator:** Your prompt should now look like this:
```
(venv) user@computer:~/KDSH$
```

The `(venv)` at the start means the virtual environment is active! ✅

### 3.4 Upgrade pip (Python Package Manager)

```bash
pip install --upgrade pip
```

**What this does:** Updates the tool that installs Python packages.

### 3.5 Install All Dependencies

This is the big step! Paste:

```bash
pip install -r requirements.txt
```

**What this does:** Installs all 20+ libraries needed for the project (PyTorch, Transformers, etc.)

**⏱️ Time:** This takes **5-15 minutes** depending on internet speed. Be patient!

**After it finishes, verify success:**

```bash
python -c "import torch; print('✅ PyTorch installed:', torch.__version__)"
```

You should see a version number printed.

---

## Step 4: Run the Project

### 4.1 Start Jupyter Lab

Your project runs as a Jupyter Notebook. Paste:

```bash
jupyter lab
```

**What happens:**
- Jupyter will start a web server
- You'll see a URL like: `http://localhost:8888/?token=abc123...`
- Copy this URL

### 4.2 Open in Your Browser

1. Open your web browser (Chrome, Edge, Firefox, etc.)
2. **Paste the URL** you copied from step 4.1
3. Press **Enter**

Jupyter Lab should open! 🎊

### 4.3 Run the Main Project

1. In the Jupyter file explorer (left sidebar), find **`main.ipynb`**
2. **Double-click** on it to open the notebook
3. Click the **▶️ Play button** or press **Shift + Enter** to run cells one by one

**Follow the on-screen instructions!** The notebook will guide you through the analysis.

### 4.4 Stop When Done

1. In your Ubuntu terminal, press **Ctrl + C** (twice if needed)
2. Type: `deactivate` to turn off the virtual environment

---

## Troubleshooting

### ❌ "WSL is not installed"

**Solution:**
1. Open PowerShell as Administrator
2. Run: `wsl --install -d Ubuntu-22.04`
3. Restart your computer

### ❌ "Python command not found"

**Solution:** Make sure you're typing `python3.12`, not just `python`

```bash
python3.12 --version
```

### ❌ "Virtual environment won't activate"

**Solution:** Make sure you're in the KDSH folder and run:

```bash
source venv/bin/activate
```

### ❌ "pip install fails with permission error"

**Solution:** Make sure your virtual environment is activated (you see `(venv)` in your prompt), then try again:

```bash
source venv/bin/activate
pip install -r requirements.txt
```

### ❌ "Jupyter Lab won't open in browser"

**Solution:**
1. Check the URL in your terminal carefully
2. Make sure you copied the entire URL including `?token=...`
3. Try a different browser

### ❌ "Out of disk space"

**Solution:** The PyTorch libraries are large (~5 GB). Delete unnecessary files or check disk space:

```bash
df -h
```

### ❌ "CUDA/GPU related errors"

**Solution:** The project can run on CPU (slower but works). Errors about GPU usually don't stop the program.

---

## Quick Reference Cheat Sheet

```bash
# Activate virtual environment (run this every time you open a new terminal)
source venv/bin/activate

# Check Python version
python3.12 --version

# Install new packages
pip install package_name

# Start Jupyter Lab
jupyter lab

# Deactivate virtual environment (when you're done)
deactivate

# Navigate to project folder
cd /mnt/c/Users/YourUsername/Desktop/Hackathon/KDSH
```

---

## What Each File Does

| File | Purpose |
|------|---------|
| `main.ipynb` | **👈 Run this!** The main analysis notebook |
| `requirements.txt` | List of all Python packages needed |
| `config.py` | Settings and configuration |
| `models/` | AI model files (transformers, embedder, etc.) |
| `pipeline/` | Data processing pipeline scripts |
| `utils/` | Helper functions |
| `Dataset/` | Sample data for testing |

---

## Getting Help

If something goes wrong:

1. **Read the error message** - it usually tells you what's wrong
2. **Check the Troubleshooting section** above
3. **Restart Ubuntu** - sometimes this fixes things:
   ```bash
   exit
   ```
   Then open Ubuntu again from the Windows Start menu.

4. **Restart the virtual environment:**
   ```bash
   deactivate
   source venv/bin/activate
   ```

---

## Next Steps

Once everything is installed and working:

1. **Open `main.ipynb`** in Jupyter Lab
2. **Read the notebook comments** - they explain what each step does
3. **Run cells one by one** using Shift + Enter
4. **Look at the outputs** to understand the project better

---

## System Requirements Summary

| Component | Requirement |
|-----------|------------|
| **OS** | Windows 10/11 |
| **Python** | 3.12 |
| **Virtual Environment** | venv (included with Python) |
| **RAM** | 8GB minimum, 16GB recommended |
| **Disk Space** | 10GB available |
| **GPU** | Optional (RTX 4070 recommended for speed, but CPU works) |

---

## Estimated Time

- WSL Installation: **10-15 minutes**
- Python Installation: **5-10 minutes**
- Project Setup: **5-15 minutes** (first time, depending on internet)
- Dependencies Installation: **10-20 minutes**
- **Total: ~45-60 minutes** (first time only)

---

**Good luck! 🚀 You've got this!**

If the notebook runs successfully, you're all set. Congratulations on setting up the KDSH project!

---

*Last updated: January 7, 2026*
