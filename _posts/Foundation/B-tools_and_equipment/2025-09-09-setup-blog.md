---
title: "How to Set Up Developer Tools with Windows WSL + Hacks"
description: "Step-by-step blog-style guide for installing WSL, Ubuntu 24.04, development tools, and running project hacks."
author: "Michelle Ji"
date: 2025-09-09
tags: ["Windows", "WSL", "Ubuntu", "VSCode", "Developer Setup", "Hacks", "JavaScript", "Games"]
---

# 🚀 How to Set Up Developer Tools with Windows WSL

Welcome! In this guide, I’ll walk you step-by-step through installing **Windows Subsystem for Linux (WSL)** with **Ubuntu 24.04**, setting up your developer tools, and preparing your system for coding in Python, JavaScript, Git, and more.  

---

## 🖥️ Step 1: Open Windows Terminal
- Pin **Windows Terminal** to your taskbar for quick access.  

---

## 🛠️ Step 2: Install WSL and Ubuntu
Run this command in your Windows Terminal:  

```sh
wsl --install -d Ubuntu-24.04

---

## 🐧 Step 3: Launch Ubuntu
To start Ubuntu any time, run:

wsl

---

## 📂 Step 4: Create Your Project Folder
Inside Ubuntu:

mkdir opencs
cd opencs

---

## 🔑 Step 5: Configure Git
Set up Git credential manager so you don't have to enter your password every time:

git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"

---

## 📥 Step 6: Clone the Student Project
Clone the repository from GitHub:

git clone https://github.com/Open-Coding-Society/student.git
cd student/

---

## ⚙️ Step 7: Activate Your Environment
Run the setup scripts:

./scripts/activate_ubuntu.sh   # enter your Ubuntu password
./scripts/activate.sh          # enter Git UID + email
./scripts/venv.sh

---

## ✅ Step 8: Verify Your Setup
Check that your tools are installed correctly:

python --version
pip --version
ruby -v
bundle -v
gem --version
git config --global --list

---

## 🔄 Step 9: Restarting a Session
Every time you open a new terminal:

cd opencs/student
source venv/bin/activate
code .

---

## **Hacks Setup and Code Walkthrough**

Now that your tools are ready, let's explore the **Hacks project** 

---

## 📂 Step 10: Open Student and Pages Directory
From the Ubuntu terminal:

cd student/pages
Open the file 2025-09-03-background-lesson.ipynb in VSCode.

---

## 🖼️ Step 11: Frontmatter Images
In the notebook you’ll see frontmatter images defined in YAML. These set up your character sprite and the background.

sprite: /images/platformer/sprites/flying-ufo.png
background: /images/platformer/backgrounds/alien_planet1.jpg

---

## 📥 Step 12: Obtain Assets via Terminal
Run these commands in the terminal to download assets:

mkdir -p hacks
wget https://raw.githubusercontent.com/Open-Coding-Society/pages/refs/heads/main/hacks/background.md -O hacks/background.md

mkdir -p images/platformer/sprites
wget https://raw.githubusercontent.com/Open-Coding-Society/pages/refs/heads/main/images/platformer/sprites/flying-ufo.png -O images/platformer/sprites/flying-ufo.png

mkdir -p images/platformer/backgrounds 
wget https://raw.githubusercontent.com/Open-Coding-Society/pages/refs/heads/main/images/platformer/backgrounds/alien_planet1.jpg -O images/platformer/backgrounds/alien_planet1.jpg

---

## ✏️ Step 13: Edit the Hack Files in VSCode
Open these files in VSCode:

hacks/background.md

Change "opencs" → "base"

Remove forward slashes on image names

images/platformer/sprites/flying-ufo.png

images/platformer/backgrounds/alien_planet1.jpg

---

## 🖌️ Step 14: Add Comments Everywhere

Use //for single line, /* */ for multi-line

---

## 🎮 Step 15: Make Your Own Scene by swapping background and sprite images.

Save a background image to backgrounds in VSCode
Save a image of an object with a transparent background to sprites in VSCode

Replace the sprite and background URL in the background.md to the URL's of your new images (make sure to delete everything before images in the URL)

Commit your changes and run make in your local host