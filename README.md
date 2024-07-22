# ASB Docker Compose

This project provides a Docker Compose setup for running an Automated Swap Backend (ASB) for XMR-BTC atomic swaps, along with its dependencies including Monero, Bitcoin Core, and Electrs.

## Overview

The setup includes the following services:

1. monerod: full monero node
2. monero-wallet-rpc: monero wallet rpc server which manages the monero wallet of the asb
3. bitcoind: full bitcoin node
4. electrs: electrum server which connects to bitcoind and gives the asb access to the bitcoin blockchain
5. [asb](https://github.com/comit-network/xmr-btc-swap/) (automated swap backend): faciliates the atomic swaps by handling cryptography, networking and connecting to the bitcoin and monero blockchain

## Prerequisites

- Docker
- Docker Compose

## Files

Depending on want network you want to run on, either navigate into the `mainnet` or `testnet` directory.

1. `.env`: Contains environment variables for port configurations.
2. `config_mainnet.toml` / `config_testnet_stagenet.toml`: Configuration file for the asb service.
3. `docker-compose.yml`: Defines the multi-container Docker application.

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
   docker-compose up -d
   ```

## Configuration

### Environment Variables

The `.env` file contains the following port configurations:

- `MONEROD_PORT`: 18081 (monero daemon RPC port)
- `MONERO_WALLET_RPC_PORT`: 18083 (monero wallet RPC port)
- `BITCOIND_RPC_PORT`: 8332 (bitcoin Core RPC port)
- `BITCOIND_P2P_PORT`: 8333 (bitcoin Core P2P port)
- `ELECTRS_PORT`: 50001 (electrs RPC port)
- `ASB_PORT`: 9939 (automated swap backend port)

### ASB Configuration

The `config_mainnet.toml` / `config_testnet_stagenet.toml` file contains the main configuration for the ASB service. Key settings include:

- `rendezvous_point`: You can add or remove multi addresses of rendezvous points here at which your asb will register
- `ask_spread`: This defines the market rate your asb will sell it's XMR for. For example: a value of `0.01` means your asb will sell Monero at 1% above market rate
- `min_buy_btc`, `max_buy_btc`: Change these to modify how much your asb is willing to swap at a time

## Usage

To start the services / to update docker images to latest version:
```
docker-compose up -d
```

To stop the services:
```
docker-compose down
```

To view logs:
```
docker-compose logs -f
```

## Data Persistence

The setup uses Docker volumes for data persistence:

- `mainnet_monerod-data` / `stagenet_monerod-data`
- `mainnet_monero-wallet-rpc-data` / `stagenet_monero-wallet-rpc-data`
- `mainnet_bitcoind-data` / `testnet_bitcoind-data`
- `mainnet_electrs-data` / `testnet_electrs-data`
- `mainnet_asb-data` / `stagenet_testnet_asb-data`

These volumes ensure that blockchain data and other persistent information are retained between container restarts.
