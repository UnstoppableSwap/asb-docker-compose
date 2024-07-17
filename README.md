# ASB Docker Compose

This project provides a Docker Compose setup for running an Automated Swap Backend (ASB) for XMR-BTC atomic swaps, along with its dependencies including Monero, Bitcoin Core, and Electrs.

## Overview

The setup includes the following services:

1. Monero daemon (monerod)
2. Monero wallet RPC (monero-wallet-rpc)
3. Bitcoin Core (bitcoind)
4. Electrs (electrs)
5. Automated Swap Backend (asb)

## Prerequisites

- Docker
- Docker Compose

## Files

Depending on want network you want to run on, either navigate into the `mainnet` or `testnet` directory.

1. `.env`: Contains environment variables for port configurations.
2. `config_mainnet.toml`: Configuration file for the asb service.
3. `docker-compose.yml`: Defines the multi-container Docker application.

## Setup

1. Clone this repository:
   ```
   git clone https://github.com/UnstoppableSwap/asb-docker-compose.git
   cd asb-docker-compose
   ```

2. Review and modify the `.env` file if you want to change any port numbers.

3. Review and modify the `config_mainnet.toml` file if you need to adjust any ASB configurations.

4. Start the services:
   ```
   docker-compose up -d
   ```

## Configuration

### Environment Variables

The `.env` file contains the following port configurations:

- `MONEROD_PORT`: 18081 (Monero daemon RPC port)
- `MONERO_WALLET_RPC_PORT`: 18083 (Monero wallet RPC port)
- `BITCOIND_RPC_PORT`: 8332 (Bitcoin Core RPC port)
- `BITCOIND_P2P_PORT`: 8333 (Bitcoin Core P2P port)
- `ELECTRS_PORT`: 50001 (Electrs RPC port)
- `ASB_PORT`: 9939 (Automated Swap Backend port)

### ASB Configuration

The `config_mainnet.toml` file contains the main configuration for the ASB service. Key settings include:

- `rendezvous_point`: You can add or remove multi addresses of rendezvous points here at which your asb will register
- `ask_spread`: This defines the market rate your asb will sell it's XMR for. For example: a value of `0.01` means your asb will sell Monero at 1% above market rate
- `min_buy_btc`, `max_buy_btc`: Change these to modify how much your asb is willing to swap at a time

## Usage

To start the services:
```
docker-compose up -d
```

To stop the services:
```
docker-compose down
```

To view logs:
```
docker-compose logs
```

## Data Persistence

The setup uses Docker volumes for data persistence:

- `mainnet_monerod-data`
- `mainnet_monero-wallet-rpc-data`
- `mainnet_bitcoind-data`
- `mainnet_electrs-data`
- `mainnet_asb-data`

These volumes ensure that blockchain data and other persistent information are retained between container restarts.