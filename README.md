# (Geth + Prysm): Ethereum Node on Docker Compose

This repository provides a Docker Compose configuration to set up a full Ethereum node, including both an execution client (Geth) and a consensus client (Prysm). This setup allows you to run a complete Ethereum node on your local machine or server, enabling you to participate in the Ethereum network, validate transactions, and interact with smart contracts.

**Geth + Prysm** is a popular combination for running an Ethereum node, where Geth serves as the execution client responsible for processing transactions and maintaining the Ethereum state, while Prysm acts as the consensus client that manages the Beacon Chain and handles Proof of Stake consensus.

## 1. Prerequisites

Create the environment variables in the `.env` file:

```bash
# .env
ETHEREUM_WALLET=0x0336fE9f1AcAC4ecf3a09e6C6dEcef8B554098dF # replace with your Ethereum wallet address
PUBLIC_IP=123.123.123.123 # replace with your public IP address to allow other nodes to connect to your node
```

## 2. Starting the Node

To start the Ethereum node, run the following command in the terminal:

```bash
docker compose up -d
```

This command will pull the necessary Docker images, create the containers for both the execution and consensus clients, and start them in detached mode.

## 3. Contributing

[<img src="https://img.shields.io/badge/Donate%20ETH-Copy-informational?logo=ethereum&style=flat-square" alt="Click to copy ETH Address">](https://etherscan.io/address/0x0336fE9f1AcAC4ecf3a09e6C6dEcef8B554098dF)

![](0x0336fE9f1AcAC4ecf3a09e6C6dEcef8B554098dF.png)

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://buymeacoffee.com/sandmann)

## Ethereum Node Architecture

``` text
                     ┌────────────────────────────────┐
                     │          ETHEREUM NODE         │
                     │ (the machine running services) │
                     └────────────────────────────────┘
                                    │
                    ┌───────────────┴───────────────────┐
                    │                                   │
┌───────────────────▼───────────────────┐  ┌────────────▼──────────────────────┐
│            EXECUTION CLIENT           │  │         CONSENSUS CLIENT          │
│     (Geth, Nethermind, Erigon…)       │  │ (Lighthouse, Prysm, Teku, Nimbus) │
│                                       │  │                                   │
│ • Runs the EVM                        │  │ • Maintains Beacon Chain state    │
│ • Processes transactions              │  │ • Applies PoS consensus           │
│ • Maintains Ethereum state + logs     │  │ • Manages slots/epochs            │
│ • Exposes JSON-RPC (eth_*)            │  │ • Coordinates validators          │
└───────────────────┬───────────────────┘  └──────────┬────────────────────────┘
                    │                       Engine API (Bidirectional JSON-RPC)
                    │
                    ▼
            Synchronized communication
                    │
                    │
┌───────────────────▼────────────────────────────────────────────────────────────┐
│                                 VALIDATOR                                      │
│               (process managed by the consensus client)                        │
│                                                                                │
│ • Proposes blocks                                                              │
│ • Signs attestations                                                           │
│ • Follows instructions from the consensus client                               │
│ • **Requires 32 ETH stake**                                                    │
│ • NO MANDATORY REQUIREMENT (can run as a non-validator)                        │
└────────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      │
                        ┌─────────────┴─────────────┐
                        │   EXTERNAL SERVICES       │
                        │ (e.g., block explorers,   │
                        │ monitoring tools, wallets)│
                        └───────────────────────────┘
```

## References

- [Ethereum Node Architecture](https://ethereum.org/en/developers/docs/nodes-and-clients/)
- [Geth Documentation](https://geth.ethereum.org/docs/)
- [Prysm Documentation](https://docs.prylabs.network/docs/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)