# ASB Docker Compose

This project provides a Docker Compose setup for running an Automated Swap Backend (ASB) for XMR-BTC atomic swaps, along with its dependencies including Monero, Bitcoin Core, and Electrs.

## Overview

The docker setup includes the following services:

| Service | Description | Docker Container Name (Mainnet) | Docker Container Name (Testnet) |
|---------|-------------|-------------------|-------------------|
| monerod | Full Monero node | `mainnet_monerod` | `stagenet_monerod` |
| monero-wallet-rpc | Monero wallet RPC server which manages the Monero wallet of the ASB | `mainnet_monero-wallet-rpc` | `stagenet_monero-wallet-rpc` |
| bitcoind | Full Bitcoin node | `mainnet_bitcoind` | `testnet_bitcoind` |
| electrs | Electrum server which connects to bitcoind and gives the ASB access to the Bitcoin blockchain | `mainnet_electrs` | `testnet_electrs` |
| [asb](https://github.com/comit-network/xmr-btc-swap/) | Automated Swap Backend (ASB): facilitates the atomic swaps by handling cryptography, networking and connecting to the Bitcoin and Monero blockchain | `mainnet_asb` | `stagenet_testnet_asb` |

## Prerequisites

- Docker
- Docker Compose

## Files

Depending on which network you want to run on, navigate into either the `mainnet` or `testnet` directory. Each directory contains the following files:

| File | Mainnet | Testnet/Stagenet | Description |
|------|---------|------------------|-------------|
| Environment file | `.env` | `.env` | Contains environment variables for port configurations |
| ASB configuration | `config_mainnet.toml` | `config_testnet_stagenet.toml` | Configuration file for the ASB service |
| Docker Compose file | `docker-compose.yml` | `docker-compose.yml` | Defines the multi-container Docker application |

## Setup

1. Clone this repository:
   ```
   git clone https://github.com/UnstoppableSwap/asb-docker-compose.git
   cd asb-docker-compose
   ```

2. Review and modify the `.env` file if you want to change any port numbers.

3. Review and modify the `config_mainnet.toml` / `config_testnet_stagenet.toml` file if you need to adjust any ASB configurations.

4. Start the services:
   ```
   docker compose up -d
   ```

## Configuration

### Environment Variables

The `.env` file contains the following port configurations:

| Service | Mainnet Port | Testnet/Stagenet Port | Description |
|---------|--------------|------------------------|-------------|
| `MONEROD_PORT` | 18081 | 38081 | Monero daemon RPC port |
| `MONERO_WALLET_RPC_PORT` | 18083 | 38083 | Monero wallet RPC port |
| `BITCOIND_RPC_PORT` | 8332 | 18332 | Bitcoin Core RPC port |
| `BITCOIND_P2P_PORT` | 8333 | 18333 | Bitcoin Core P2P port |
| `ELECTRS_PORT` | 50001 | 60001 | Electrs RPC port |
| `ASB_PORT` | 9939 | 9839 | Port used by the ASB for network communication with peers using Libp2p |

### ASB Configuration

The `config_mainnet.toml` / `config_testnet_stagenet.toml` file contains the main configuration for the ASB service. Key settings include:

| Setting | Description |
|---------|-------------|
| `rendezvous_point` | You can add or remove multi addresses of rendezvous points here at which your ASB will register |
| `ask_spread` | This defines the market rate your ASB will sell its XMR for. For example: a value of `0.01` means your ASB will sell Monero at 1% above market rate |
| `min_buy_btc`, `max_buy_btc` | Change these to modify how much your ASB is willing to swap at a time |

## Usage

### To start the services and to update docker images to latest version:
```
docker compose pull
docker compose up -d
```

### To stop the services:
```
docker compose down
```

### To view logs:
```
docker compose logs -f
```

### To get a list of all swaps:
```
# If you're running on mainnet
compose exec mainnet_asb asb --config=/asb-data/config_mainnet.toml history

# If you're running on testnet
docker compose exec stagenet_testnet_asb asb --testnet --config=/asb-data/config_stagenet_testnet.toml history
```

### To export the wallet descriptor of the internal Bitcoin wallet:

**Mainnet**
```bash
# Stop the ASB service. This releases the lock on the Bitcoin wallet
docker compose stop mainnet_asb

# Run the export command
docker compose run --rm mainnet_asb asb --config=/asb-data/config_mainnet.toml export-bitcoin-wallet

# Start the ASB again
docker compose start mainnet_asb
```
**Testnet:**
```bash
# Stop the ASB service. This releases the lock on the Bitcoin wallet
docker compose stop stagenet_testnet_asb

# Run the export command
docker compose run --rm stagenet_testnet_asb asb --testnet --config=/asb-data/config_stagenet_testnet.toml export-bitcoin-wallet

# Start the ASB again
docker compose start stagenet_testnet_asb
```
## Data Persistence
The setup uses Docker volumes for data persistence:

| Volume Name (Mainnet) | Volume Name (Testnet) | Description |
|-------------|-------------|-------------|
| `mainnet_monerod-data` | `stagenet_monerod-data` | Monero blockchain data |
| `mainnet_monero-wallet-rpc-data` | `stagenet_monero-wallet-rpc-data` | Monero wallet data |
| `mainnet_bitcoind-data` | `testnet_bitcoind-data` | Bitcoin blockchain data |
| `mainnet_electrs-data` | `testnet_electrs-data` | Electrs server data |
| `mainnet_asb-data` | `stagenet_testnet_asb-data` | ASB persistent data |


These volumes ensure that blockchain data and other persistent information are retained between container restarts.
