# Financially sovereign: Bitcoin Full Node (part 1)

*Not your seed, not your coins. Not your node, not your rules. **Don\'t trust, verify!***

To become truly financially sovereign, I need to make sure that everyone is playing according to the rules. For this, the Bitcoin Full Node is needed.

Running your own node also enhances privacy and allows you to run the Lightning Network node.

## The problem

When Bitcoin wallet connects to the network it usually won\'t download the whole blockchain and thus won\'t verify all the transactions. It will use some third party server to do this job. This poses both privacy and security concerns. A trusted third party sees the addresses that you are checking for balance and in theory, can even give you false data.

The solution is to run your own Bitcoin Full Node and setup the wallet to use this node instead.

## Raspiblitz

There are many different solutions for running a node. In this article, I recommend using [Raspiblitz](https://github.com/rootzoll/raspiblitz) because:

1. Hardware for it is cheap (200€ / 230$)
2. It is easy to setup.
3. It supports [Lightning Network](https://lightning.network/) out of the box.
4. It is actively maintained and supported by its developers.
5. It has a big and great community.
6. It provides huge utility by supporting many services.

## Hardware

Raspiblitz runs on Raspberry Pi and I recommend this minimal setup:

- Raspberry PI 4 with Power Adapter
- Heat-sink with active cooling
- SSD 1TB (cheap option is 2.5 SATA with [external HDD Box](https://www.axagon.eu/en/produkty/ee25-xa6))
- Micro SD card, 16GB (SDHC)
- **USB stick for backups**

Raspiblitz can be assembled with a nice display, but it is not needed in my opinion.

Shopping lists [Amazon](https://github.com/rootzoll/raspiblitz#hardware-needed) (from Raspiblitz documentation) plus backup USB stick.

## Installation

Raspiblitz has amazing [documentation directly on Github](https://github.com/rootzoll/raspiblitz). It is a step-by-step guide with screenshots.

Make sure you [setup your node to run behind Tor](https://github.com/rootzoll/raspiblitz#running-behind-tor). It is just about selecting the right option in the Raspiblitz setup. You don\'t want to anyone know you have a full node at home.

After installation and Bitcoin blockchain sync (it will take days) you have Bitcoin Full Node and Lightning Node ready.

## Backup

You have the seed for the LN wallet written down. This is the backup for on-chain funds. However, once you open some channels, funds will be locked in those channels. In case the SSD fails, you need **the latest** backup of those channels.

However, using the whole channel\'s state (the file called `channel.db`) is tricky. You may accidentally broadcast the old state and get punished for it.

> **Broadcasting old channel state (old backup) is seen by Lightning Network as a fraud attempt.** It will most likely result in a Penalty Transaction that will sweep funds from your side of a channel. For more info see [this article](https://blog.bitmex.com/lightning-network-justice/) [5].

To go around this problem, the [Static Channel Backup](https://wiki.ion.radar.tech/tutorials/troubleshooting/static-channel-backups) was introduced.

It comes with a tradeoff: you recover your funds, but your channels will be closed. It can be costly because opening and especially force-closing channels may cost some significant on-chain fees. But in the contrast of loosing all your off-chain funds, it is a great deal.

Raspiblitz offers some options on how to do Static Channel Backup:

1. Raspiblitz keeps the backup file `channel.backup` both on the SD card and on the main SSD by default.
2. You can configure Raspiblitz to [keep backup on a USB stick](https://github.com/rootzoll/raspiblitz/blob/v1.6/README.md#c-local-backup-target-usb-thumbdrive) plugged into a Raspberry Pi.
3. You can configure Raspiblitz to [upload encrypted backup into Dropbox](https://github.com/rootzoll/raspiblitz/blob/v1.6/README.md#a-dropbox-backup-target). But be aware that by doing this, you are exposing to Dropbox the fact that you are running the LN node. Using the fake account and Tor is advised.

Raspiblitz has [great documentation about this](https://github.com/rootzoll/raspiblitz/blob/v1.6/README.md#backup-for-on-chain---channel-funds) and a [nice video](https://www.youtube.com/watch?v=5wi6l9jRVQs) explaining it.

## Testing the Backup

It is crucial to have backups. **But if you haven\'t tested your backup you don\'t know if you really have it.**

To test the seed:

1. Fund the wallet with some very small amount of Bitcoin.
2. Erase the LN wallet by going to the menu and `[REPAIR]`→`[RESET-LND]`.
3. Then recover the Wallet from the seed.
4. Verify that you have recovered the correct wallet by checking the balance and funding transaction from step 1.

The only suitable way how to check that `channel.backup` is ok is to check that the file changes (on USB or in Dropbox) after the channel state has changed.

Truly testing it by doing the backup process is not possible because it would force-close all your channels in the process.

## Lightning Network

Raspiblitz comes with [Ride the Lightning](https://github.com/Ride-The-Lightning/RTL) app (RTL) that allows managing the node via the browser. It will be accessible via IP on your local network or via onion address over Tor allowing you to manage your node remotely.

The next step is to fund the wallet with some non-trivial amount of Bitcoin and start opening channels. Make sure they are big enough (I would recommend a minimum of 1M sats).

Preferring those "big" channels is good:

1. Because the on-chain fees can be quite high making such channel uneconomic allocation of funds. This can be especially a problem in a situation when you need to service the channel (for example rebalance it by on-chain swap).
2. Every channel has also some funds reserved for its force-closing transaction. The nodes will be updating the reserved amount regularly according [[6]](https://github.com/lightningnetwork/lightning-rfc/blob/master/05-onchain.md) to the state of the mempool and can be quite high in busy times.

### Inbound capacity

By opening a channel we have all sats on your side and it means we can send, but we cannot receive.

The easiest way how to get some inbound is to open a big channel and spend some funds on goods and services. This will move some sats on the other side of a channel allowing you to receive payments.

Here is a [list of options](https://github.com/openoms/lightning-node-management/blob/master/CreateInboundLiquidity.md) on how to get inbound liquidity.

### Using Lightning Node from the Mobile Wallet

The next step is to connect the LN wallet to your mobile phone to be able to spend sats in a grocery store. Raspiblitz supports two main wallets: [Zap](https://zaphq.io/) and [Zeus](https://zeusln.app/).

- Official Raspiblitz [documentation](https://github.com/rootzoll/raspiblitz#mobile-connect-mobile-wallet)
- Nice [video tutorial](https://www.youtube.com/watch?v=XStiTJosklY) for Zeus which is also applicable for Zap, because the main concept is the same.

## Connect on-chain wallet to your node (Electrum Server)

Electrum server is a service that allows the wallet to access the Bitcoin blockchain.

> **This is the part, where you became truly sovereign.** By configuring your wallet to communicate with your own Bitcoin Full Node hidden behind Tor, over the Tor, you became your own bank. There is no trusted third party between you and the Bitcoin network. **You just became the very part of the Bitcoin network.** [[7]](https://docs.wasabiwallet.io/using-wasabi/BitcoinFullNode.html#the-importance-of-running-a-full-node)

Again, there is a [great video about it](https://www.youtube.com/watch?v=AiosKK_TA7w&t=276s) and [documentation](https://github.com/rootzoll/raspiblitz#electrum-rust-server) covering it.

Wallets **supporting it**:

- Run **[Electrum Wallet](https://electrum.org/)** by using a command like
  ```
  ./electrum-4.0.9-x86_64.AppImage --oneserver --server xxxxxxxxxxxxxxxxxxxxxxx.onion:50002:s --proxy socks5:127.0.0.1:9150
  ```
  
  You can get the exact command for your node from the Raspiblitz menu: `[ELECTRS]`→`[CONNECT]`. **The Tor browser needs to be running to serve as a Tor proxy.** If you have [`torsocs` installed](https://linuxconfig.org/install-tor-proxy-on-ubuntu-20-04-linux) and use them as a proxy, use port `9050` instead of `9150`. Check [Electrum documentation](https://electrum.readthedocs.io/en/latest/tor.html) for more info about connecting Electrum Wallet to a Tor node.
- In **[Phoenix Wallet](https://phoenix.acinq.co/)** go to settings and navigate `[General]`→`[Electrum server]`, click the `[Set server]` button, and paste the onion address provided by Raspiblitz. The port must be changed to `50001` (for more info see [phoenix/issues/44](https://github.com/ACINQ/phoenix/issues/44#issuecomment-717315660)).
- In **[Wasabi Wallet](https://www.wasabiwallet.io/)** you just need to [set the onion address](https://docs.wasabiwallet.io/using-wasabi/BitcoinFullNode.html#using-an-already-existing-remote-bitcoin-full-node). of your Bitcoin Full node. **It is different from the Electrum server** and can get it by `bitcoin-cli getnetworkinfo` command. Wasabi then uses your full node to download the block instead of using a random one.

Wallets **NOT supporting it 🔔 but hopefully soon will**:

- [TREZOR Suite](https://github.com/trezor/trezor-suite) has it is on [the roadmap](https://github.com/trezor/trezor-suite/issues/2737).

## Blockchain Explorer

Pasting your transaction IDs into random blockchain explorer is possibly quite dangerous. You have to trust the third party that:

1. They are presenting you with the correct data.
2. They will not pair your IP with the transaction and sell this information to a blockchain analysis company (can be mitigated by Tor).

The solution to this is to run your own [Blockchain Explorer](https://github.com/janoside/btc-rpc-explorer) on your Bitcoin Full Node. All that is needed is to [turn it on](https://github.com/rootzoll/raspiblitz#btc-rpc-explorer) in your Raspiblitz.

## Conclusion

Running the full node and using it to accessing the Bitcoin network allows you to became sovereign by cutting off third parties. You can connect your wallets directly to your node and browse transactions in the Blockchain Explorer that is solely under your control.

Having the Lightning Network node allows you to connect to the network without relying on third parties (and their big nodes) like [ACINQ](https://acinq.co/) (authors of the otherwise great Phoenix wallet). You can control fees and open channels to "create paths" for your future transactions.

In future articles, we will focus on the [Lightning Loop](https://github.com/rootzoll/raspiblitz#lightning-loop), [CoinJoin](https://github.com/rootzoll/raspiblitz#joinmarket), and the [Lightning Pool](https://github.com/rootzoll/raspiblitz#lightning-pool).

## Sources and further readings

1. [Raspiblitz on Github](https://github.com/rootzoll/raspiblitz)
2. [Andreas M. Antonopoulos: Bitcoin Q&A: Why Running a Node is Important](https://www.youtube.com/watch?v=oX0Yrv-6jVs)
3. [Lightning Network (Part 3) – Where Is The Justice?](https://blog.bitmex.com/lightning-network-justice/)
4. [Lightning Network RFC](https://github.com/lightningnetwork/lightning-rfc)
5. [Wasabi Wallet: The importance of running a full node](https://docs.wasabiwallet.io/using-wasabi/BitcoinFullNode.html#the-importance-of-running-a-full-node)
6. [bitcoin.it | Full node](https://en.bitcoin.it/wiki/Full_node)
