## Nockchain
Nockchain is a lightweight chain for heavyweight compute. It uses ZK-Proof of Work (zkPoW). Miners create a ZK-Proof (ZKP) of a fixed puzzle computation, then hash the ZKP, and earn $NOCK based on their computation power.

The team has released a Public Testnet to run a ***local testnet node*** and a ***testnet miner***, to explore how Nockchain works before Mainnet goes live.

## $NOCK Details
- Mining starts May 19th.
- Total Supply: 2^32 nocks (around 4.29 billion).
- Fair launch: 100% of $NOCK will be issued to Miners.
- $NOCK is used to pay for blockspace on Nockchain.

## Mainnet Network Details
- Mainnet to be launched in 19th May.
- 10-min block times (like Bitcoin)
- For Testnet, we run *"Miner"* and a *"simulated local testnet"* with a **fake genesis block**, to connect our Miner to it.
- Once Mainnet live, **public peers** are published, so we just need to run Miner alone. It connects to public peers.

## $NOCK Issuance Details
- The **earlier** you start mining, the **BIGGER** rewards you are going to earn.

![image](https://github.com/user-attachments/assets/ead8fbd3-4199-452a-94c4-68b371c21aad)

## Hardware Requirements
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
    <td><code>16 GB</code></td>
    <td><code>6 cores or higher</code></td>
    <td><code>50-200 GB SSD</code></td>
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

# CLI Setup
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
Building may take more than 15 minutes.
```bash
# Eden ZKVM, Jock language
make build-hoon-all

# Rust-based node and drivers
make build
````

## Step 5: Setup Wallet
* Set PATH:
```bash
export PATH="$PATH:$(pwd)/target/release"
```
* Create wallet:
```bash
wallet keygen
```
* Save `memo`, `private key` & `public key` of your wallet.
> Note: After every terminal restart, Ensure you execute these two commands before executing wallet commands again: `cd nockchain` & `export PATH="$PATH:$(pwd)/target/release"`.  By doing this, you won't get Error: `wallet: command not found`.

## Step 6: Configure Nodes
Your Node's configuration is in `Makefile`
Open `Makefile`:
```bash
nano Makefile
```
* `MINING_PUBKEY`: Replace your wallet `public key` with its value.
* `Ports`: By default, Nodes use ports `3005` and `3006`. If these ports are occupied on your system, modify them in the node configuration.
* To save: `Ctrl+X` + `Y` + `Enter`

### Step 7: Run Leader Node
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

## Usefull commands
### Wallet commands:
> Note: After every terminal restart, Ensure you execute these two commands before executing wallet commands again: `cd nockchain` & `export PATH="$PATH:$(pwd)/target/release"`.  By doing this, you won't get Error: `wallet: command not found`.

General Wallet Command:
```bash
wallet --nockchain-socket ./test-leader/nockchain.sock
```
Wallet Balance:
```bash
wallet --nockchain-socket ./test-leader/nockchain.sock balance
```
* It looks good. `~` is like a 0 and balance will be 0 until you mine a block.

![image](https://github.com/user-attachments/assets/52550e55-21b2-4625-84f9-3250eca367f5)

* [Official repo](https://github.com/zorp-corp/nockchain/blob/master/crates/wallet/README.md) for more Wallet commands.

### Screen commands:
Ensure screens do not overlap. Before opening or switching to another screen, minimize or close the current screen.
```console
# Return leader screen (leader logs)
screen -r leader

# Return leader screen (follower logs)
screen -r follower

# Minimize screen
Press: CTRL + A + D

# Screens list
screen -ls

# Stop Node when inside a screen
Press: Ctrl + C

# Kill and Remove screen when outside a screen (replace NAME)
screen -XS NAME quit
```

---

Well, We just tested a Nockchain Miner node on the testnet, which is *NOT incentivized*. This was an experiment to prepare for the Mainnet launch on **May 19**. The team might update the docs with new features, and the Mainnet setup may ir may not differ.

---

# GUI Setup (App)
There's a new project called **Nockpool**, I think they are building a GUI-based Node so you can easily open a wallet on Windows and Mine $NOCK.
* Register for Wallet email update: https://swps.io/wallet
* Register for Nockpool email update: https://swps.io/nockpool

![image](https://github.com/user-attachments/assets/6f58647d-2255-4ebb-839c-eeb539cac258)
