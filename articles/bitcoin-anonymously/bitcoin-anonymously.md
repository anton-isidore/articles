# Bitcoin anonymously
Bitcoin is simply **NOT anonymous**! How to hide your coins then?

## MUST READ!
- [Privacy at bitcoin.it wiki](https://en.bitcoin.it/wiki/Privacy)

## Run your node, avoid 3rd pariest
In the article "[Financially sovereign: Building the Bank](articles/financially-soveregin-building-the-bank/financially-soveregin-building-the-bank.md)" is described how to build your bank without trusting third parties.

In a nutshell:
- Buy yourself a [TREZOR](https://trezor.io/)
- Run your own Bitoin full node (for example [Raspiblitz](https://github.com/rootzoll/raspiblitz))
- Use TREZOR with the [Electrum](https://electrum.org/) wallet, connected to your own fullnode
- Use your own blockchain explorer (running on your own full node) and never paste your address or transaction ID into any online service

## Practice coin control
1. Never reuse addreses
2. Label all your transactions and UTXOs so you have data to avoid doxxing yourself
3. Be carefull when you consolidate coins (= spending 2 UTXO in one transaction). This publicly proofs that both UTXOs were owned by the same person (you). Ideally you should never consolidate coins.
4. Be aware that when you spend leftovers from previous transactions, the trail of those unspend leftovers is visible in the blockchain (it's called [peeling chain](https://en.bitcoin.it/wiki/Privacy#Change_address_detection)).
5. Because of previsou point, it is good to spend whole UTXO.
6. Small leftovers (sometimes called dust) can be donated to charities and opensource projects. (Becase consolidatong them will doxx you and mixing them is expensive.)
7. Do not send ROUND values. When you send 0.01 BTC it is obious what is payment and what is unspend change.

## Use Lightning Network
1. Run your own Lightning node with for example [Raspiblitz](https://github.com/rootzoll/raspiblitz)
2. Create bunch of channels with **MIXED** bitcoins and use **WHOLE** UTXO (do not create doxxic change)
3. Inboud liquidity can be obtained:
   1. Openning channel and spending coins
   2. [Buy channel from LNBIG](https://lnbig.com/#/open-channel)
   3. [LightningTo.me](https://lightningto.me/) offers inbound channel after you create some number of your own channels
   4. [Coincharge.io](https://coincharge.io/en/lightning-liquidity/) is openning reciprocal channels if you open channels to them
   5. ...exmplore articles in the next section for more options
4. Connect wallets like Zeus/Zap or create private channel to your node from your favourite LN Wallet
5. Enyjoy your privacy

#### Articles and documentation on: How to get Inbound liquidity
- https://openoms.gitbook.io/lightning-node-management/createinboundliquidity
- https://coincharge.io/en/lightning-liquidity/


## Mix coins
- [CoinJoin](https://en.bitcoin.it/wiki/CoinJoin)

### Whirlpool (Samourai)
- [samouraiwallet.com](https://samouraiwallet.com/)
- [Tool to calculate fees](https://www.whirlpoolfees.com/)

### Wasabi Wallet
- [wasabiwallet.io](https://www.wasabiwallet.io/)

### JoinMarket
- [JoinMarker on Github](https://github.com/JoinMarket-Org/joinmarket-clientserver)
- Run [JoininBox](https://github.com/openoms/joininbox) on [Raspiblitz](https://github.com/rootzoll/raspiblitz)
- Cheapest option, because you can run Yield Generator and providie liquidity. Others will mix your coin for free (or even for small fee).


