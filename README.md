## Nockchain
Nockchain is a lightweight chain for heavyweight compute. It uses ZK-Proof of Work (zkPoW). Miners create a ZK-Proof (ZKP) of a fixed puzzle computation, then hash the ZKP, and earn $NOCK based on their computation power.

The team has released a Public Testnet to run a ***local testnet node*** and a ***testnet miner***, to explore how Nockchain works before Mainnet goes live.

## $NOCK Details
- Total Supply: 2^32 nocks (around 4.29 billion).
- Fair launch: 100% of $NOCK will be issued to Miners.
- $NOCK is used to pay for blockspace on Nockchain.


## Mainnet Details
- Mainnet to be launched at 19th May.
- For Testnet, we run *"Miner"* and a *"simulated local testnet"* with a **fake genesis block**, to connect our Miner to it.
- Once Mainnet live, **public peers** are published, so we just need to run Miner alone. It connects to public peers.

## $NOCK Issuance Details
- The **earlier** you start mining, the **BIGGER** rewards you are going to earn.

![image](https://github.com/user-attachments/assets/ead8fbd3-4199-452a-94c4-68b371c21aad)

## Hardware Requirements
* zkPoW is **CPU-mining** currently
* 16GB of RAM
* 200GB of available disk space
* System: Local PC or VPS
* Windows Users: Install Linux Ubuntu on your Windows using this [Guide](https://github.com/0xmoei/Install-Linux-on-Windows)
* VPS: Recommended crypto-payment VPS provider to [Purchase](https://my.hostbrr.com/order/forms/a/NTMxNw==) or Visit [VPS Beginner Guide](https://github.com/0xmoei/Linux_Node_Guide/)

> Note: Miners are initially CPU-based for users and will eventually move to GPU and ASIC.
>
> Zorp as a for-profit labs company behind Nockchain will be selling private, closed-source software to mining partners, possibly even GPU-based to help bootstrap the initial security and proofrate of the network.
>
> Meaning they will likely dominate block rewards over the vast majority of people, but yet the protocol might potentially be rewarding for early users.

---

## CLI Setup
### Step 1: Install Dependecies
* Update Packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
* Install Packages:
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```
* Rust:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
```bash
source $HOME/.cargo/env
```

* Docker (For Mainnet, we might choose Docker setup)
```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world

sudo systemctl enable docker
sudo systemctl restart docker
```

### Step 2: Clone NockChain Repo
```bash
git clone https://github.com/zorp-corp/nockchain

cd nockchain
```

### Step 3: Install Choo (Jock/Hoon Compiler)
```bash
make install-choo
```
* This compiles `choo`, the Nock-based compiler used for running Jock programs and ZKVM applications.
  
### Step 4: Build
```bash
# Eden ZKVM, Jock language support
make build-hoon-all

# Rust-based node and drivers
make build
````

### Step 5: Run Leader Node
* Open a screen:
```bash
screen -S leader
```
* Start a **Leader Node** (Testnet Node for fake genesis block):
```bash
make run-nockchain-leader
```

### Step 6: Run Follower Node:
* Open a screen
```bash
screen -S leader
```

* Start a **Follower Node** (Miner Node for connecting to other peers):
```bash
make run-nockchain-follower
```

## Wallet

```bash
export PATH="$PATH:$(pwd)/target/release"
```
