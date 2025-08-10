---
title: "Docker to Detection: Setting Up Wazuh SIEM for Beginners"
date: 2025-06-14
description: "How I set up a Wazuh-based SIEM using Docker, detected a real CVE, and connected my Linux machine as an agent."
author: "Arsh Imtiaz"
tags: ["SIEM", "Cybersecurity", "Wazuh", "Docker", "Homelab"]
---

![Wazuh](/images/wazuh-header-image.jpg)

Setting up a SIEM (Security Information and Event Management) system might sound intimidating if you’re just getting started, but it’s a rewarding project that gives you real insight into how cybersecurity professionals monitor and detect threats. I recently built a Wazuh-based SIEM entirely with Docker and connected my personal Linux workstation as an agent. In this post, I’ll walk you through the process with practical commands for both Arch Linux and Debian users.

---

> **Wazuh** started as a fork of OSSEC, one of the first open-source host-based intrusion detection systems, but has evolved into a full-blown security platform with log analysis, vulnerability detection, and compliance monitoring.

---

## System Overview

Here’s what I used:

- Host OS: Arch Linux (but I include Debian commands too, since many readers might be on either)
- Deployment method: Docker (because containers make complex stacks easier to manage)
- Architecture: Single-node setup running Manager, Indexer, and Dashboard on one machine
- Agent: My own Linux workstation
- Use case: Personal monitoring and vulnerability detection in a home lab

---

## Step 1: Installing Docker and Docker Compose

Before we get started, make sure Docker is installed and running. If you don’t have it, here’s how:

**On Arch Linux:**

```bash
sudo pacman -S docker docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
```

**On Debian or Ubuntu:**

```bash
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
```
---

> Docker was originally released in 2013 and revolutionized how developers package and deploy applications. Today, it’s an essential tool in cybersecurity labs for simulating complex environments quickly.

---

## Step 2: Cloning the Wazuh Docker Repository

Wazuh maintains an official Docker repository with all the configurations needed for different deployments. Grab the single-node setup like this:

```bash
git clone https://github.com/wazuh/wazuh-docker -b v4.12.0
cd wazuh-docker/single-node
```

---

## Step 3: Generating SSL Certificates

To secure communication between the components, you’ll need to generate SSL certificates. First, create the directory:

```bash
mkdir -p config/wazuh_indexer_ssl_certs/
```

Next, create the configuration files. Here’s the **config/certs.yml** file content:

```yaml
nodes:
  - name: wazuh.indexer
    type: indexer
  - name: wazuh.manager
    type: manager
  - name: wazuh.dashboard
    type: dashboard
```

And the **generate-indexer-certs.yml** file:

```yaml
version: '3.3'
services:
  generator:
    image: wazuh/wazuh-certs-generator:0.0.2
    hostname: wazuh-certs-generator
    volumes:
      - ./config/wazuh_indexer_ssl_certs/:/certificates/
      - ./config/certs.yml:/config/certs.yml
```

Then run:

```bash
docker-compose -f generate-indexer-certs.yml run --rm generator
```

If you get errors about overwriting files, just clear the folder and try again:

```bash
rm -rf config/wazuh_indexer_ssl_certs/*
docker-compose -f generate-indexer-certs.yml run --rm generator
```

---

## Step 4: Starting the Wazuh Stack

With everything set, launch the whole Wazuh environment:

```bash
docker-compose up -d
```

This will bring up the manager, indexer, and dashboard.

---

## Step 5: Accessing the Web Dashboard

Open your browser and head to:

```bash
https://<your-server-ip>
```
This would be localhost if you're setting it up locally.

---

  
The default login credentials are:

- Username: `admin`
- Password: `SecretPassword` (or check the `.env` file in the repo)

Expect a warning about the self-signed certificate. This is normal. You can safely proceed for now.

---

## Adding Your Own Machine as an Agent

Wazuh’s strength lies in monitoring endpoints. I connected my Arch Linux workstation as an agent, but these steps work the same on Debian.

### Download and install the agent:

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-agent-4.12.0-linux-x86_64.tar.gz
tar -xvzf wazuh-agent-4.12.0-linux-x86_64.tar.gz
cd wazuh-agent-4.12.0
sudo ./install.sh
```

### Register the agent with the manager:

```bash
sudo /var/ossec/bin/agent-auth -m <wazuh-manager-ip>
sudo systemctl enable --now wazuh-agent
```

After a short while, your agent will show up in the dashboard, forwarding logs and security events.

---

> The first version of Wazuh was released in 2015, and it has since become one of the most widely used open-source security platforms worldwide.

**You're now done!** You should be able to explore the dashboard and learn all the features Wazuh has to offer.


---

## Real-World Detection: CVE-2025-4598

Shortly after getting everything running, Wazuh’s Vulnerability Detector flagged a real systemd vulnerability on my machine (CVE-2025-4598). This was a practical example of how such tools help spot risks before attackers do.

*Don’t worry, I patched it right away. Wouldn’t want to make it too easy for you hackers, right?*

---

## Why This Matters

Setting this up wasn’t just an academic exercise. I now have a hands-on understanding of log collection, normalization, and alerting. In fields like automotive cybersecurity, knowing what’s happening on endpoints and spotting anomalies early is crucial—and this kind of setup lets you practice those skills.

---

## What’s Next?

Here are some things you can try for yourself now that you've got the hang of setting it up:
- Crafting custom detection rules tailored to your environment
- Simulating attacks to validate your alerts
- Adding Windows or other OS agents
- Expanding to multi-node deployments for scalability

---

## Final Thoughts

Wazuh is a fantastic project for anyone looking to deepen their cybersecurity skills. Using Docker simplifies the setup, and the dashboard delivers meaningful insights out of the box. Whether you’re a beginner or an experienced professional, experimenting with Wazuh builds a solid foundation for real-world security monitoring.

Thanks for reading! Feel free to reach out if you want to discuss or share your own setups.
