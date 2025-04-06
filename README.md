🚀 Cuckoo Node CPU Setup (Stable Diffusion Miner Without GPU)

This guide walks you through running a Cuckoo Stable Diffusion miner node in CPU mode, ideal for VPS environments like Contabo where GPU access is not available.

✅ Tested on: Ubuntu 22.04, root access, Docker 24+, Docker Compose v2.34+

🧰 Requirements

Ubuntu 20.04 / 22.04 VPS

Root user access

Docker (v20+)

Docker Compose Plugin

⚙️ Automatic Installation (Recommended)

📄 Step 1: Download and Run the Setup Script

curl -O https://raw.githubusercontent.com/Vevivo/cuckoo-node-cpu/main/setup.sh
chmod +x setup.sh
sudo ./setup.sh

At the end of the script, you'll be prompted to enter your ETH private key.

⚙️ Manual Installation (Optional)

🧱 1. Install Docker and Compose

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

📦 2. Clone the Repository

git clone https://github.com/cuckoo-network/stable-diffusion-miner-docker.git
cd stable-diffusion-miner-docker

🛠️ 3. Launch Node in CPU Mode

# First, stop any existing Docker containers (cleanup)
docker compose down

Then launch the node using your ETH_PRIVATE_KEY:

ETH_PRIVATE_KEY="0xYOUR_PRIVATE_KEY" SD_URL="http://auto-cpu:7860" make start-cpu

🔍 4. Monitor Logs

docker logs -f webui-docker-relay-node-1

If you see lines like below, everything is working fine:

check pending tasks
no pending tasks

🌐 5. Access Web UI

Open in browser:

http://[YOUR_SERVER_IP]:7860

🧼 To Stop the Node

make stop

This stops the compose profile only. To shut down everything:

docker compose down

✨ Notes

Since this runs on CPU, it may take time to receive tasks.

You can monitor logs to verify task assignments.

auto-cpu is used instead of auto, so make sure SD_URL is set accordingly.

