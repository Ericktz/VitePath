# Nodes in Vite Network

In the Vite network, nodes are categorized into Full Nodes and Snapshot Block Producer (SBP) aka Super nodes in the crypto scene. The following sections introduces the installation of FullNodes and SBP.

## Fullnodes 
The fullnodes are integral to the network, tasked with maintaining a comprehensive copy of the blockchain ledger, facilitating the transmission and receipt of transactions, and validating all network transactions. Additionally, a Full Node can engage in SBP elections and voting processes. It provides external HTTP/WEBSOCKET APIs and features a local command line interface, which can be enabled via configuration settings.

## Supernodes 
The SBPs are integral to blockchains that use a Delegated Proof of Stake (DPOS) consensus mechanism, similar to a representative democracy where elected officials validate transactions and maintain the public ledger. In Vite, a variant of DPOS called Hierarchical Delegated Proof of Stake (HDPOS) is used, leveraging the Block Lattice architecture, a type of Directed Acyclic Graph (DAG).

- **Local Consensus:** Each user and smart contract has its own chain. Delegated Consensus Groups, composed of selected full nodes, determine the state of these chains.
- **Global Consensus:** At this level, the Snapshot Consensus Group manages the Snapshot Chain, which stores essential information like account balances and Merkle Tree roots, acting as an auditor for local transactions.

Supernodes are part of the Snapshot Consensus Group, with the top 100 eligible to become SBPs. The top 25 voting Supernodes become Snapshot Block Producers (SBPs), have a greater probability of creating blocks compared to the remaining 75 SBPs, and are responsible for producing blocks on the Snapshot Chain, thus enhancing security in the DAG architecture.


Running SBPs can be a beneficial use of VITE coins, especially during crypto winters.

For a step-by-step guide on setting up a Fullnodes/SBP, refer to the next pages.

Should you have any questions regarding this topic, please feel free to make an issue.

