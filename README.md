# GUIDE_RomeProtocol-



# RomeProtocol Installation Guide

This guide explains how to download and set up RomeProtocol repositories and Docker images for version `v0.3.0`.

---

## **Step 1: Clone the Rome Setup Repository**
Use the following command to clone the `rome-setup` repository:
```bash
git clone --branch v0.3.0 --single-branch https://github.com/rome-labs/rome-setup.git
```

---

## **Step 2: Download Docker Images (for Ubuntu)**
If you are running on **Ubuntu**, you can directly pull the pre-built Docker images:
```bash
docker pull romelabs/rollup-op-geth:v0.3.0
docker pull romelabs/rome-apps:v0.3.0
docker pull romelabs/rome-evm:v0.3.0
```

---

## **Step 3: Build Docker Images Locally (if not on Ubuntu)**

If you are not using Ubuntu, you will need to build the Docker images locally.

### Clone Required Repositories
```bash
git clone --branch v0.3.0 https://github.com/rome-labs/rome-apps.git
git clone --branch v0.3.0 https://github.com/rome-labs/rome-sdk.git
git clone --branch v0.3.0 https://github.com/rome-labs/rome-evm.git
git clone --branch v0.3.0 https://github.com/rome-labs/rome-rollup-clients.git
git clone --branch v0.3.0 https://github.com/rome-labs/op-geth.git
```

### Build Docker Images
Run the following commands to build the required Docker images:

1. **Build the `rome-apps` image**:
   ```bash
   docker buildx build -t romelabs/rome-apps:v0.3.0 -f rome-apps/docker/Dockerfile .
   ```

2. **Build the `rollup-op-geth` image**:
   ```bash
   docker buildx build -t romelabs/rollup-op-geth:v0.3.0 -f ./rome-rollup-clients/op-geth/Dockerfile .
   ```

---

## **Step 4: Configuration and Startup**
1. Navigate to the `rome-setup` directory:
   ```bash
   cd rome-setup
   ```

2. Follow the setup instructions provided in the `rome-setup` repository to configure and start the Docker containers.

---

## **Additional Resources**
For more detailed instructions, visit the official [RomeProtocol Documentation](https://github.com/rome-labs).

---

# Guide: Register Your L2 on RomeProtocol Testnet

This guide will walk you through registering your L2 (Layer 2) rollup on the RomeProtocol testnet using a new Solana keypair (rollup owner keypair) and a Chain ID.

---

## **Step 1: Generate Solana Keypair**
A Solana keypair is required to initialize your L2. This keypair will act as the **rollup owner keypair**, so ensure it is stored securely.

### Command to Generate the Keypair:
1. Navigate to the `rome-setup/docker` directory:
   ```bash
   cd rome-setup/docker
   ```

2. Run the following command to generate the keypair:
   ```bash
   solana-keygen new -o keys/rollup-owner-keypair.json --no-bip39-passphrase --force
   ```

   - **`-o keys/rollup-owner-keypair.json`**: Specifies the output file where the keypair will be stored.
   - **`--no-bip39-passphrase`**: Disables passphrase protection for this keypair.
   - **`--force`**: Overwrites the file if it already exists.

3. **Important**: Save the keypair file securely, as it will be used for managing your rollup.

---

## **Step 2: Submit the Registration Request**

After generating your keypair, proceed with the registration.

1. Visit the L2 registration page:  
   [https://dapps.testnet.romeprotocol.xyz/l2-register](https://dapps.testnet.romeprotocol.xyz/l2-register)

2. Provide the **public key** of the generated Solana keypair during registration.  
   The public key can be obtained by running:
   ```bash
   solana-keygen pubkey keys/rollup-owner-keypair.json
   ```

3. Follow the instructions on the page to complete the registration form, including specifying a **Chain ID**.

---

## **Step 3: Confirm Registration**

- Once the registration request is submitted, your chain will be registered.
- You will see a success message on the website confirming the completion.

---

### Notes:
- Ensure the keypair file is backed up securely to prevent losing access to your rollup.
- Use only testnet credentials and chains for this process.


# Guide: Initializing Your L2 on RomeProtocol Testnet

This step-by-step guide will help you create the environment required to initialize your L2 rollup on the RomeProtocol testnet. You will generate secrets, keypairs, configure settings, and create the initial balance.

---

## **Step 1: Generate a JWT Secret**
The JWT secret is used by Geth for authentication. Run the following command to generate it:

```bash
openssl rand -hex 32
```

**Example Output**:  
`78f54d165d7996252e4664a4d3049b56b9b0b0e5d4c9e50c0f6887205766aafb`

Save this value securely, as it will be needed later.

---

## **Step 2: Generate Solana Keypairs**
You need two Solana keypairs: one for the **Rhea service** and another for the **Proxy service**.

1. Navigate to the directory:
   ```bash
   cd rome-setup/docker
   ```

2. Generate the **Rhea keypair**:
   ```bash
   solana-keygen new -o keys/rhea-sender.json --no-bip39-passphrase --force
   ```

3. Generate the **Proxy keypair**:
   ```bash
   solana-keygen new -o keys/proxy-sender.json --no-bip39-passphrase --force
   ```

Store these keypair files securely.

---

## **Step 3: Set Environment Variables**
Set up the required environment variables for your rollup. Run the following commands:

```bash
export CHAIN_ID=1002
export GENESIS_ADDRESS=0xf0e0CA2704D047A7Af27AafAc6D70e995520C2B2
export GENESIS_PRIVATE_KEY=241bfd22ba3307c78618a5a4c04f9adbd5c87d633df8d81cfb7c442004157aba
export GENESIS_BALANCE=1000000000000000000000000
export JWT_SECRET=78f54d165d7996252e4664a4d3049b56b9b0b0e5d4c9e50c0f6887205766aafb
export GASOMETER_HOST=http://proxy:9090
export SOLANA_RPC=https://apieu.devnet.romeprotocol.xyz
export PROGRAM_ID=RD2Gg7Lcnv62XmRHAzxh6fQQfMRzHtN5LeKPVBhYU5S
export GETH_HOST=http://localhost:3000
export AIRDROP_TITLE="Testnet Token Airdrop"

export ROME_EVM_TAG=v0.3.0
export RHEA_TAG=v0.3.0
export PROXY_TAG=v0.3.0
export GETH_TAG=v0.3.0
export CLI_TAG=v0.3.0
```

Update Solana configuration:
```bash
solana config set -u https://apieu.devnet.romeprotocol.xyz
```

If you are running on a remote server, set `GETH_HOST` to your domain name (e.g., `https://rollup.testnet.romeprotocol.xyz`) instead of `http://localhost:3000`.

---

## **Step 4: Update Configuration Files**
### Check the Current Solana Slot
Run the following command to get the current Solana slot:
```bash
solana slot
```

Use this value to set the `start_slot` in the `proxy-config.yml` and `rhea-config.yml` files located in `rome-setup/docker/cfg`.

### Update `rhea-config.yml`
Example configuration for `rhea-config.yml`:
```yaml
chain_id: 1002

start_slot: <CURRENT_SOLANA_SLOT>
solana:
  rpc_url: "https://apieu.devnet.romeprotocol.xyz"
solana_indexer:
  rpc_url: "https://apieu.devnet.romeprotocol.xyz"

program_id: "RD2Gg7Lcnv62XmRHAzxh6fQQfMRzHtN5LeKPVBhYU5S"

payers:
  - payer_keypair: "/opt/rhea-sender.json"
    fee_recipients:
      - 0xB136AB5B69A8059f5cB30A8B5418F5e43d8f40e8

geth_engine:
  geth_engine_addr: "http://geth:8551"
  geth_engine_secret: "78f54d165d7996252e4664a4d3049b56b9b0b0e5d4c9e50c0f6887205766aafb"
geth_indexer:
  geth_http_addr: "http://geth:8545"
  geth_poll_interval_ms: 100
```

### Update `proxy-config.yml`
Example configuration for `proxy-config.yml`:
```yaml
chain_id: 1002

start_slot: <CURRENT_SOLANA_SLOT>
solana:
  rpc_url: "https://apieu.devnet.romeprotocol.xyz"
  commitment: "confirmed"

program_id: "RD2Gg7Lcnv62XmRHAzxh6fQQfMRzHtN5LeKPVBhYU5S"

proxy_host: "0.0.0.0:9090"

payers:
  - payer_keypair: "/opt/proxy-sender.json"
    fee_recipients:
      - 0xB136AB5B69A8059f5cB30A8B5418F5e43d8f40e8
```

---

## **Step 5: Create Initial Balance**
### Fund Rollup Owner
Use [Solana Faucet](https://faucet.solana.com) to airdrop SOL to your rollup owner account. If the faucet fails, contact the RomeProtocol team on Discord or Telegram.

### Run the Balance Creation Command
After funding the rollup owner, run:
```bash
docker-compose up create_balance
```

If you encounter the error **"Signer not found, or more than one signer was found"**, ensure that:
1. The rollup owner's Solana keypair is properly funded.
2. The airdrop transaction was successful.

---

## **Next Steps**
Once the initial balance is set, your L2 environment is ready for use. Follow the remaining documentation to start deploying contracts or interacting with the rollup.  

# Guide: Setting Up an OP Geth Node and Block Explorer for RomeProtocol

This guide explains how to set up the OP Geth Node, associated services (Rhea and Light Client), and the RomeProtocol Block Explorer. 

---

## **OP Geth Node Overview**

The OP Geth Node is a non-voting RPC node for Ethereum Layer 2 (L2) transactions. It interacts with Solana to sequence transactions. Three services are required:

1. **Light Client (Proxy):** Provides gas estimates and Solana state for Ethereum transactions.
2. **Geth:** Accepts and executes Ethereum L2 transactions.
3. **Rhea:** Packages Geth transactions and submits them to Solana for sequencing.

### **Recommended System Specifications**
- **OS:** Ubuntu  
- **RAM:** 32GB  
- **CPU:** 16 cores  
- **Storage:** 1TB  

---

## **Step 1: Start Docker Containers**
Run the following services in the specified order.

### **1. Light Client (Proxy)**
The Light Client calculates gas estimates and provides an Ethereum interface for Solana transactions.

1. **Airdrop SOL to Proxy Keypair:**  
   Use the following command to get the keypair address for the faucet:
   ```bash
   solana address -k keys/proxy-sender.json
   ```
   Visit [Solana Faucet](https://faucet.solana.com) to fund the address. If you encounter issues, contact support on Discord or Telegram.

2. **Run the Light Client Docker container:**
   ```bash
   docker-compose up -d proxy
   docker logs proxy -f
   ```
   Wait for the log to display:  
   **"Starting the RPC server at 0.0.0.0:9090".**

---

### **2. Geth**
Geth accepts and executes Ethereum L2 transactions.

1. **Update Docker Volumes:**  
   If running locally, comment out the following lines in `rome-setup/docker/docker-compose.yml`:
   ```yaml
     # volumes:
     #   - /etc/letsencrypt/live/${GETH_HOST}/fullchain.pem:/etc/nginx/ssl/selfsigned.crt
     #   - /etc/letsencrypt/live/${GETH_HOST}/privkey.pem:/etc/nginx/ssl/selfsigned.key
   ```

   For remote servers, keep the lines uncommented.

2. **Run the Geth Docker container:**
   ```bash
   docker-compose up -d geth
   docker logs geth -f
   ```
   Wait for the log to display:  
   **"Server listening at http://localhost:3000".**

---

### **3. Rhea**
Rhea composes Ethereum transactions from Geth into Solana transactions.

1. **Airdrop SOL to Rhea Keypair:**  
   Use the following command to get the keypair address for the faucet:
   ```bash
   solana address -k keys/rhea-sender.json
   ```
   Fund the address via the [Solana Faucet](https://faucet.solana.com). Contact support if issues arise.

2. **Run the Rhea Docker container:**
   ```bash
   docker-compose up -d rhea
   docker logs rhea -f
   ```
   Wait for the log to display:  
   **"Polling: http://geth:8545".**

---

## **Step 2: Docker Container Overview**
| Docker Container | Purpose                    |
|-------------------|----------------------------|
| **proxy**         | Rome Light Client App      |
| **geth**          | OP Geth App               |
| **rhea**          | Rhea App                  |

---

## **Step 3: Set Up Rome Scout Block Explorer**
Rome Scout allows you to view both Ethereum (Geth) and Solana states.

### **1. Clone the Repository**
- For local machines:
  ```bash
  git clone --branch local https://github.com/rome-labs/romescout.git
  ```
- For remote servers:
  ```bash
  git clone --branch testnet https://github.com/rome-labs/romescout.git
  ```

Navigate to the Docker directory:
```bash
cd romescout/docker-compose
```

---

### **2. Update Configuration**
#### If Running on a Remote Server:
Replace `rollup.testnet.romeprotocol.xyz` with your domain in the following files:
- `romescout/docker-compose/services/nginx.yml`
- `romescout/docker-compose/envs/common-frontend.env`

#### Branding and Network Info:
Customize naming and branding in the `.env` file:
```bash
NEXT_PUBLIC_NETWORK_NAME=Rome
NEXT_PUBLIC_NETWORK_SHORT_NAME=Rome
NEXT_PUBLIC_NETWORK_ID=915817419
NEXT_PUBLIC_NETWORK_CURRENCY_NAME=Rome
NEXT_PUBLIC_NETWORK_CURRENCY_SYMBOL=ROME
NEXT_PUBLIC_NETWORK_LOGO=http://rome-public-assets.s3.us-east-1.amazonaws.com/rome-banner.png
NEXT_PUBLIC_NETWORK_ICON=http://rome-public-assets.s3.us-east-1.amazonaws.com/rome-logo.png
NEXT_PUBLIC_HOMEPAGE_PLATE_TEXT_COLOR=white
NEXT_PUBLIC_HOMEPAGE_PLATE_BACKGROUND=#5E0A60
```

---

### **3. Build and Run Block Explorer**
1. **Remove old data:** *(Skip if running for the first time)*  
   ```bash
   sudo rm -rf services/blockscout-db-data
   sudo rm -rf services/stats-db-data
   ```

2. **Build and start Docker container:**
   ```bash
   docker-compose up --build -d
   ```

3. **To stop the service:**
   ```bash
   docker-compose down
   ```

Access the block explorer:  
- **Locally:** [http://localhost:1000](http://localhost:1000)  
- **Remote server:** Replace with your domain, e.g., `https://rollup.testnet.romeprotocol.xyz:1000`.

---

## **Next Steps**
- Verify that all containers are running correctly and logs show no errors.
- Access the block explorer to monitor transactions and state updates.
- Contact the RomeProtocol team for additional support via [Discord](https://discord.gg/romeprotocol) or Telegram. 

