# 🚀 Cuckoo Node CPU Setup (Stable Diffusion Miner Without GPU)

This guide walks you through running a Cuckoo Stable Diffusion miner node in **CPU mode**, ideal for VPS environments like Contabo where GPU access is not available.

> ✅ Tested on: Ubuntu 22.04, root access, Docker 24+, Docker Compose v2.34+

---

## 🧰 Requirements

- Ubuntu 20.04 / 22.04 VPS
- Root user access
- [Docker](https://docs.docker.com/engine/install/ubuntu/) (v20+)
- [Docker Compose Plugin](https://docs.docker.com/compose/install/linux/)

---

## ⚙️ Manual Installation

### 🧱 1. Install Docker and Compose

```bash
# Remove any existing Docker versions
sudo apt remove docker docker-engine docker.io containerd runc -y

# Install dependencies
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker’s official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up the repository
echo \ 
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

### 📦 2. Clone the Repository

```bash
git clone https://github.com/cuckoo-network/stable-diffusion-miner-docker.git
cd stable-diffusion-miner-docker
```

---

### 🛠️ 3. Launch Node in CPU Mode

```bash
# First, stop any existing Docker containers (cleanup)
docker compose down
```

Then launch the node using your **ETH_PRIVATE_KEY**:

```bash
ETH_PRIVATE_KEY="0xYOUR_PRIVATE_KEY" SD_URL="http://auto-cpu:7860" make start-cpu
```

---

### 🔍 4. Monitor Logs

```bash
docker logs -f webui-docker-relay-node-1
```

If you see lines like below, everything is working fine:

```
check pending tasks
no pending tasks
```

---

### 🌐 5. Access Web UI

Open in browser:

```
http://[YOUR_SERVER_IP]:7860
```

---

## 🧼 To Stop the Node

```bash
make stop
```

> This stops the compose profile only. To shut down everything:

```bash
docker compose down
```

---

## ✨ Notes

- Since this runs on CPU, it may take time to receive tasks.
- You can monitor logs to verify task assignments.
- `auto-cpu` is used instead of `auto`, so make sure `SD_URL` is set accordingly.

---

---

## ❗️ Troubleshooting & Clarifications

- **No GPU / NVIDIA Required:** This CPU setup does **not** require an NVIDIA GPU, driver, or CUDA toolkit.
- If you get errors related to `nvidia-container-cli`, make sure you're using the correct command:
  
  ```bash
  make start-cpu
