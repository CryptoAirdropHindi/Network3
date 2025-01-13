#!/bin/bash
set -e

RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[0;33m'
CYAN='\033[0;36m'
NC='\033[0m'

curl -s https://raw.githubusercontent.com/CryptoAirdropHindi/Tools/refs/heads/main/logo.sh | bash
echo "Starting Auto Install Node Network3"
sleep 5

log() {
    local level=$1
    local message=$2
    local timestamp=$(date +"%Y-%m-%d %H:%M:%S")
    local border="-----------------------------------------------------"
    echo -e "${border}"
    case $level in
        "INFO") echo -e "${CYAN}[INFO] ${timestamp} - ${message}${NC}" ;;
        "SUCCESS") echo -e "${GREEN}[SUCCESS] ${timestamp} - ${message}${NC}" ;;
        "ERROR") echo -e "${RED}[ERROR] ${timestamp} - ${message}${NC}" ;;
        *) echo -e "${YELLOW}[UNKNOWN] ${timestamp} - ${message}${NC}" ;;
    esac
    echo -e "${border}\n"
}

check_port() {
    local port=$1
    if ss -tuln | grep -q ":$port"; then
        log "ERROR" "Port $port is already in use. Please check and free the port."
        exit 1
    else
        log "INFO" "Port $port is available."
    fi
}

get_server_ip() {
    ip addr show $(ip route | awk '/default/ {print $5}') | awk '/inet / {print $2}' | cut -d/ -f1
}

log "INFO" "Updating and upgrading system packages"
sudo apt update && sudo apt upgrade -y
log "SUCCESS" "System updated successfully"

log "INFO" "Installing required dependencies"
sudo apt install net-tools ufw -y
log "SUCCESS" "Dependencies installed successfully"

log "INFO" "Checking if port 8080 is available"
check_port 8080

log "INFO" "Allowing port 8080 through the firewall"
sudo ufw allow 8080
sudo ufw reload
log "SUCCESS" "Port 8080 is now open"

log "INFO" "Downloading node files"
cd $HOME
wget https://network3.io/ubuntu-node-v2.1.1.tar.gz
log "SUCCESS" "Download completed"

log "INFO" "Extracting node files to 'Network3' folder"
mkdir -p $HOME/Network3
tar -xzf ubuntu-node-v2.1.1.tar.gz -C $HOME/Network3 --strip-components=1
log "SUCCESS" "Files extracted successfully to 'Network3' folder"

log "INFO" "Running node setup script"
cd $HOME/Network3
sudo bash manager.sh up
log "SUCCESS" "Node setup completed successfully"

sudo bash manager.sh key
log "SUCCESS" "Save key and bind"

server_ip=$(get_server_ip)
log "INFO" "Your server IP is: $server_ip"
log "INFO" "Activate the node using this link: https://account.network3.ai/main?o=${server_ip}:8080"
