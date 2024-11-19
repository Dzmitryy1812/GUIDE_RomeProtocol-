# GUIDE_RomeProtocol-
Вот адаптированный гайд для публикации на GitHub как README.md:

---

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

This guide provides the essential steps for setting up RomeProtocol. If you encounter any issues, feel free to open an issue or submit a pull request!

--- 

