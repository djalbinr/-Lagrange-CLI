#requments:

# Update package list and install dependencies
sudo apt update && sudo apt install -y curl wget apt-transport-https ca-certificates gnupg lsb-release \
&& \
# Install Golang
wget https://go.dev/dl/go1.21.1.linux-amd64.tar.gz -O go.tar.gz && sudo tar -C /usr/local -xzf go.tar.gz \
&& \
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.bashrc && source ~/.bashrc \
&& \
# Install Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg \
&& \
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null \
&& \
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io \
&& \
# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
&& \
sudo chmod +x /usr/local/bin/docker-compose \
&& \
# Verify installations
go version && docker --version && docker-compose --version


#Golang:


# Update package list and upgrade all packages
sudo apt-get update && sudo apt-get -y upgrade \
&& \
# Download Go 1.21.9 and extract it to /usr/local
wget https://go.dev/dl/go1.21.9.linux-amd64.tar.gz && sudo tar -xvf go1.21.9.linux-amd64.tar.gz -C /usr/local \
&& \
# Set Go environment variables and update PATH
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile && export GOROOT=/usr/local/go && source ~/.profile \
&& \
# Verify Go installation
go version





#Docker and Docker Compose


# Update package list
sudo apt-get update \
&& \
# Create directory for apt keyrings
sudo install -m 0755 -d /etc/apt/keyrings \
&& \
# Download Docker GPG key and save it
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
&& \
# Adjust permissions for the Docker GPG key
sudo chmod a+r /etc/apt/keyrings/docker.gpg \
&& \
# Add Docker repository to apt sources list
echo "deb [arch=\"$(dpkg --print-architecture)\" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null \
&& \
# Update package list again after adding Docker repository
sudo apt-get update \
&& \
# Install Docker components and essential build tools
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin make gcc \
&& \
# Configure Docker to limit log file size
echo '{ "log-opts": { "max-size": "10m", "max-file": "100" } }' | sudo tee /etc/docker/daemon.json \
&& \
# Add current user to Docker group to allow running Docker without sudo
sudo usermod -aG docker $USER \
&& \
# Refresh group membership to apply changes
newgrp docker \
&& \
# Restart Docker service to apply configuration changes
sudo systemctl restart docker




#Clone and setup the latest version of Lagrange CLI repository on your machine.


# Clone the Lagrange Labs CLI repository
git clone https://github.com/Lagrange-Labs/client-cli.git \
&& \
# Set CGO flags for the build process
export CGO_CFLAGS="-O -D__BLST_PORTABLE__" \
&& \
export CGO_CFLAGS_ALLOW="-O -D__BLST_PORTABLE__" \
&& \
# Navigate to the cloned directory
cd client-cli \
&& \
# Download Go module dependencies
go mod download \
&& \
# Install necessary build tools
sudo apt install -y make gcc \
&& \
# Build the project
make build
