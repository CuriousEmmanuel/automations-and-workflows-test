# automations-and-workflows-test
1. Start/Stop Docker Itself (System-Level)

# Start Docker daemon
sudo systemctl start docker

# Stop Docker daemon
sudo systemctl stop docker

# Restart Docker
sudo systemctl restart docker

# Enable Docker to auto-start on boot
sudo systemctl enable docker

# Disable Docker auto-start
sudo systemctl disable docker

# Check Docker status
sudo systemctl status docker

🔧 Initial Goal

Install official Docker CE + plugins (buildx, compose) on Kali Linux x64.
✅ Step-by-Step with Errors Encountered
1. ⚙️ Added Docker Repo for Kali

    Attempted to use:

https://download.docker.com/linux/debian kali-rolling

❌ Error:

The repository ... kali-rolling Release does not have a Release file.

✅ Fix:

    Changed kali-rolling to bookworm in Docker repo:

        https://download.docker.com/linux/debian bookworm stable

2. 📦 Attempted to Install Docker Components

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    ❌ Error:

Tried to start delayed item ... runc ... but failed

✅ Fix:

    Cleaned apt cache and fixed broken packages:

        sudo apt clean
        sudo rm -rf /var/lib/apt/lists/*
        sudo apt update
        sudo apt --fix-broken install -y

3. 🔐 Encountered GPG Warnings

    Warnings (ignored safely):

    Policy will reject signature within a year

    Related to newer GPG policy — not blocking.

4. 🧱 Unpacking Errors for Plugins

    ❌ Error:

dpkg error while processing docker-compose-plugin: file already exists

Cause:

    You had docker-compose (2.26.1) installed

    Docker plugin version 2.38.2 tried to install over same file

✅ Fix (surgical):

    Forced overwrite of .deb without removing old package:

        sudo dpkg -i --force-overwrite /var/cache/apt/archives/docker-compose-plugin_*.deb
        sudo apt -f install

5. ⚠️ Final Install Fix

    Plugin docker-buildx-plugin failed post-install

    ✅ Fix:

        Ran:

        sudo apt -f install

        Completed setup

6. ✅ Validated Installation

    Ran:

    docker run hello-world
    docker info
    docker compose version
    docker buildx version

    ✅ Everything working as expected

🧰 Final Setup Includes:

    Docker CE v28.3.2

    Docker Compose Plugin v2.38.2

    Docker Buildx Plugin v0.25.0

    Containers working

    Compose YAML working

    System startup controlled via systemctl
