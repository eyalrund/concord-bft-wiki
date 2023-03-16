Welcome to the concord-bft wiki!

Project Concord uses Byzantine fault-tolerant consensus protocols to deliver a functioning distributed trust system: one that is both “safe” and “alive.” Concord is a generic state machine replication library that can handle malicious (Byzantine) replicas.

While BFT technology and its application are well-understood, BFT-based systems require substantial communication between nodes and, thus, don’t scale well—an impediment for enterprise-scale blockchain environments. Project Concord solves this problem by simplifying and streamlining communication between nodes, enabling greater scalability while increasing overall network throughput.

Project Concord’s BFT engine obtains significant scaling improvements via three major advances:

1. It uses a linear communication consensus protocol—many other BFT consensus protocols (including [PBFT](https://dl.acm.org/citation.cfm?id=571640)) require quadratic communication
2. It exploits optimism to provide a common case fast-path execution (like [Zyzzyva ](https://dl.acm.org/citation.cfm?id=1658358)and with a correct view change protocol)
3. It uses modern cryptographic algorithms (BLS threshold signatures)
More details about the BFT consensus protocol can be found in the recent paper, “[SBFT: A Scalable Decentralized Trust Infrastructure for Blockchains](https://arxiv.org/pdf/1804.01626.pdf).

## General Resources
[Official Site](https://projectconcord.io/) - read about the library, News & Events and community. 

## Community

Follow us at: [@project_concord](https://www.twitter.com/@project_concord)
Chat with us at: [concordbft.slack.com](https://concordbft.slack.com/)
Request a Slack invitation via <concordbft@gmail.com>.

## License

concord-bft is available under the [Apache 2 license](LICENSE).