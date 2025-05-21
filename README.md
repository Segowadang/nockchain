## Nockchain
Nockchain is a lightweight chain for heavyweight compute. It uses ZK-Proof of Work (zkPoW). Miners create a ZK-Proof (ZKP) of a fixed puzzle computation, then hash the ZKP, and earn $NOCK based on their computation power.

The team has released a Public Testnet to run a ***local testnet node*** and a ***testnet miner***, to explore how Nockchain works before Mainnet goes live.

---

## $NOCK Details
- Genesis Mainnet and NOCK Mining starts May 21th.
- 10-min block times (like Bitcoin).
- Total Supply: 2^32 nocks (around 4.29 billion).
- Fair launch: 100% of $NOCK will be issued to Miners.
- $NOCK is used to pay for blockspace on Nockchain.

---

## Nock Mining Dashboard
This community-driven dashboard, [**NockStats**](https://nockstats.com/) has many good features like: Wallet balance, Trading, Tokenomics, Calculator, etc.

---

## How to Mine
* The mining terms is exactly same a Bitcoin PoW mining.
* More computational power = More rewards
* More miners on the network = Higher hashrate = More difficulty to mine rewards

### Method 1: Solo (CLI):
* You run a solo CPU-based Miner Node on your system which will certainly need a powerful hardware.
* You can follow [CLI Setup](https://github.com/0xmoei/nockchain/blob/main/README.md#cli-miner-setup) steps.

### Method 2: Pool & Method 3: GUI
* You join a pool (same as bitcoin mining), share your computational power and earn rewards in a shared pool of rewards.
* Official mining method is CLI introduced by the team, and no official Pool or GUI mining programs introduced.
* But new community-driven *Projects* and *Pools* are poping up.
* **GUI Setup (App)**: There's a new project called [**Nockpool**](https://swps.io/nockpool), I think they are building a GUI-based Node so you can easily open a wallet on Windows, join a pool, and Mine $NOCK.

![image](https://github.com/user-attachments/assets/6f58647d-2255-4ebb-839c-eeb539cac258)

---

# CLI Miner Setup
## Hardware Requirements
Hardware requirement is highly speculative since no one knows how Mainnet-launch goes.

<table>
  <tr>
    <th colspan="3">Miner is CPU-based initially</th>
  </tr>
  <tr>
    <td>RAM</td>
    <td>CPU</td>
    <td>Disk</td>
  </tr>
  <tr>
    <td><code>64 GB</code></td>
    <td><code>6 cores or higher</code></td>
    <td><code>100-200 GB SSD</code></td>
  </tr>
</table>

* more CPU = more hashrate = more chances
* We still can't say that's enough, because it's just a testnet environment and we need to wait until mainnet.

---

## OS Requirements

* **Windows Users:** Install Linux Ubuntu on your Windows using this [Guide](https://github.com/0xmoei/Install-Linux-on-Windows)
* **VPS:** Recommended crypto-payment VPS provider to [Purchase](https://my.hostbrr.com/order/forms/a/NTMxNw==) or Visit [VPS Beginner Guide](https://github.com/0xmoei/Linux_Node_Guide/)

> Note: Miners are initially CPU-based for users and will eventually move to GPU and ASIC.
>
> Zorp as a for-profit labs company behind Nockchain will be selling private, closed-source software to mining partners, possibly even GPU-based to help bootstrap the initial security and proofrate of the network.
>
> Meaning they will likely dominate block rewards over the vast majority of people, but yet the protocol might potentially be rewarding for early users.

---

## CLI Installation Steps
### Step 1: Install Dependecies
* Update Packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
* Install Packages:
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev libclang-dev llvm-dev -y
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

### Step 2: Delete Old Repo and Wipe All Data
```bash
rm -rf nockchain
rm -rf .nockapp
```

### Step 3: NockChain Repo
```bash
git clone https://github.com/zorp-corp/nockchain

cd nockchain
```
  
### Step 4: Build
* Copy `.env` file from the example one:
```bash
cp .env_example .env
```

* Build: (takes time depending on your hardware.)
```bash
make install-hoonc
```
```bash
make build
```
```bash
make install-nockchain-wallet
```
```bash
make install-nockchain
```

## Step 5: Setup Wallet
Make sure you are in `nockchain` directory.
* **Set PATH:**
```bash
export PATH="$PATH:$(pwd)/target/release"
```
* **Create wallet:**
```bash
nockchain-wallet keygen
```
* Save `memo`, `private key` & `public key` of your wallet.
> Note: After every terminal restart, Ensure you execute these two commands before executing wallet commands again: `cd nockchain` & `export PATH="$PATH:$(pwd)/target/release"`.  By doing this, you won't get Error: `wallet: command not found`.

* Replace the value of `MINING_PUBKEY=` in `.env` with your own Public Key:
```bash
nano .env
```
To save: Press `Ctrl + X`, `Y` & `Enter`.

## Step 6: Backup/Import Wallet Keys
Backup wallet keys:
```bash
nockchain-wallet export-keys
```
* This will save your keys to a file called `keys.export` in the current directory.

Import wallet keys:
```bash
nockchain-wallet import-keys --input keys.export
```
* Make sure `keys.export` is in your current directory

## Step 7: Get BTC Mainnet RPC -- (Mine Genesis Block)
* If you want to join from **Genesis Block #0**, then you need to add `---btc-url` flags to your Node command and Connect your Miner to a BTC RPC.
* Team will publish the Bitcoin block height at which the genesis block will start being mine. We need a Bitcoin Node since no one have access to the hash of the Genesis Block before it's mined.
* If you don't want to join **Genesis Block #0**, Miner will work and "pass" the genesis block verification even if you do not connect your miner to a Bitcoin node.

1- Get your free and private BTC Mainnet RPC from third-party platforms like: [Ankr](https://www.ankr.com/rpc/?utm_referral=LqL9Sv86Te), [QuickNode](https://dashboard.quicknode.com/), or [Alchemy](https://dashboard.alchemy.com/). 

2- Check your RPC sync and connection status:
* Replace `https://rpc.ankr.com/btc/{your_rpc_token}` with your RPC url.
```
curl -X POST https://rpc.ankr.com/btc/{your_rpc_token} \
-d '{
      "id": "hmm",
      "method": "getindexinfo",
      "params": []
}'
```

### Step 8: Run Miner
* Open a screen:
```bash
screen -S miner
```
* Start a Miner:
```bash
make run-nockchain
```
* Wait for it to install.
* To minimize screen:  `Ctrl` + `A` + `D`

### Screen Useful Commands:
Ensure screens do not overlap. Before opening or switching to another screen, minimize or close the current screen.
```console
# Return screen
screen -r miner

# Minimize screen
Press: CTRL + A + D

# Screens list
screen -ls

# Stop Node (when inside a screen)
Press: Ctrl + C

# Kill and Remove Node (when outside a screen)
screen -XS miner quit
```

---

## FAQ
* Can I use same pubkey if running multiple miners?
  * Yes, you can use the same pubkey if running multiple miners.

* How do I change the mining pubkey?
  * Run nockchain-wallet keygen to generate a new key pair and copy the new public key to the .env file.


#

#

#


## Step 5: Configure Nodes
Your Node's configuration is in `Makefile`
Open `Makefile`:
```bash
nano Makefile
```
* `MINING_PUBKEY`: Replace your wallet `public key` with its value.
* `Ports`: By default, Nodes use ports `3005` and `3006`. If these ports are occupied on your system, modify them in the node configuration.
* To save: `Ctrl+X` + `Y` + `Enter`

### Step 6: Open ports
```console
# Allow ssh port
sudo ufw allow ssh
sudo ufw allow 22

# Enable firewall
sudo ufw enable

# Open ports
sudo ufw allow 3005/tcp
sudo ufw allow 3006/tcp
```

### Step 7: Run Leader Node
Leader Node is a fake testnet node. On mainnet Peer IDs will be replaced with Leader node, we only need to run Follower Node in the next step.
* Open a screen:
```bash
screen -S leader
```
* Start a **Leader Node** (Testnet Node for fake genesis block):
```bash
make run-nockchain-leader
```
* Wait for it to install.
* To minimize:  `Ctrl` + `A` + `D`

### Step 8: Run Follower Node:
* Open a screen
```bash
screen -S follower
```
* Start a **Follower Node** (Miner Node for connecting to other peers):
```bash
make run-nockchain-follower
```
```console
# OK Response:
I (12:18:31) "inner dumbnet cause: [%command %timer]"
I (12:18:32) nc: block by-height: [Ok(%heavy-n) Ok(1) 0]
```
* To minimize:  `Ctrl` + `A` + `D`

---
