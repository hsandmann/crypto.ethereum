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

## 3. Verifying the Node

You can verify that both the execution and consensus clients are running correctly by checking the logs:

```bash
docker compose logs -f
```

This will display the logs from both clients in real-time. Look for messages indicating that the clients are synchronizing with the Ethereum network and that they are connected to peers.

```bash
geth          | INFO [03-09|20:31:12.878] Imported new potential chain segment     number=24,622,315 hash=de7acc..3597c6 blocks=1 txs=231 mgas=26.045 elapsed=103.680ms   mgasps=251.207 triediffs=364.83MiB triedirty=213.87MiB
geth          | INFO [03-09|20:31:12.921] Chain head was updated                   number=24,622,315 hash=de7acc..3597c6 root=ab3511..24aaaf elapsed=2.058517ms
prysm-beacon  | time="2026-03-09 20:31:12.94" level=info msg="Synced new block" block=0xfa2c8e51... epoch=432979 finalizedEpoch=432977 finalizedRoot=0x9d4877a1... prefix=blockchain slot=13855354
prysm-beacon  | time="2026-03-09 20:31:12.94" level=info msg="Finished applying state transition" attestations=8 kzgCommitmentCount=5 payloadHash=0xde7acc24a71a prefix=blockchain slot=13855354 syncBitsCount=509 txCount=231
```

This will display the logs from both clients in real-time. Look for messages indicating that the clients are synchronizing with the Ethereum network and that they are connected to peers.

You can also check the synchronization status of the consensus client (Prysm) by sending a request to its API:

```bash
curl http://localhost:3500/eth/v1/node/syncing
```

This will return a JSON response indicating whether the node is currently syncing with the network or if it is fully synchronized. A response of `false` for the `is_syncing` field indicates that the node is fully synchronized and ready to participate in the Ethereum network. The response will also include information about the current slot, head block, and sync distance, which can help you monitor the synchronization progress. A response looking like this indicates that the node is fully synchronized:

```json
{
        "data": {
                "head_slot": "13855336",
                "sync_distance": "0",
                "is_syncing": false,
                "is_optimistic": false,
                "el_offline": false
        }
}
```

## Contributing

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