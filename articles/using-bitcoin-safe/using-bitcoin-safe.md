## Using Bitcoin Safe

### Onchain Bitcoin is NOT anonymous❗
I cannot stess this enought. 

#### Problem

The amount of bitcoin you have is not just a one number that you see in our wallet. 
Bitcoins in your wallet are actually stored in pieces called [UTXO](https://en.wikipedia.org/wiki/Unspent_transaction_output)
which stands for Unspent Transaction Output.

Those UTXOs have history in the blockchain which identifies them. So if you combine them in a transaction, person
who is spying on you can clearly see, that all those UTXOs are from the same person.

This process of combining UTXOs is called *consolidation* and it is huge risk if you want to stay anonymous. 
It allows the attacker to cluster UTXOs and if you did just one mistake somewhere 
(UTXO from the KYCed exchaned, buying online with your real name/address, ...) ALL your bitcoins can get exposed!

Also keep in mind that if you are spending one UTXO over many transactions you are creating something called **peel-chain**, 
if you expose your identity to just one place in this chain, there is a big chance that all 

```
1 BTC -> 0.05 BTC (buy A)
      -> 0.95 BTC (change)  -> 0.2  BTC (buy B)
                            -> 0.75 BTC (change)  -> 0.002 BTC (buy C)
                                                  -> 0.748 BTC (change)  -> 0.2   BTC (buy D)
                                                                         -> 0.548 BTC (change)   -> ...
 ```

This is how a peel-chain can look like and if you are able to identify what is the spend and what is the change 
(by amount, rounding, known merchant adddresses, ...) it is easy to identify such chain and if you expose your identity just
once in that spend chain, the whole chain can be attributed to you.

#### Solution
1. Use Lightning Network whenever possible
2. Practice Coin Control, label your UTXos
3. Avoid creating a long peel chains
   - Ideally spend whole UTXO with no change at all
   - If possible, use Lightning to fill the missing amount or refund the overpayment
4. **NEVER** consolidate, if you end up with some small UTXOs, **do NOT mixed them with other coins!**
   - rather donate them to some charity
   - or if you are greedy fuck, swap them for Lightning on [fixedfloat.com](https://fixedfloat.com/) or similar

### Use Lightning Network to spend ⚡

#### Problem
You have UTXO that you want to spend anonymously!

#### Solution
1. Open Lightning Channel to some big node using the UTXO
2. Spend whole channel capacity
3. Close the channel with the [1% of unspendable amount](https://github.com/lightning/bolts/blob/master/02-peer-protocol.md#rationale) to the address of some charity.

This will cost you ~1% (the unspendable amount in the channel) but **you will keep your anonymity** and **you will boost your karma** by supportig some good charity.

## Charities and good projects to donate to

## Charities 
1. [savelife.in.ua](https://savelife.in.ua/en/donate-en/#donate-army-crypto) 🇺🇦
 
## Projects
1. [Tor](https://donate.torproject.org/cryptocurrency/)
2. [Tasks](https://tasks.org/docs/donate/) - Great Android app for Todo-List
3. [NewPipe](https://newpipe.net/donate/)- Youtube Client for Androind (no adds, download videos, playing on background, ...)
4. [KeePassDx](https://www.keepassdx.com/#donation) - Android client for KeePass
5. [F-Droid](https://f-droid.org/en/donate/) - FOSS Androind apps repository
6. [Signal](https://www.signal.org/donate/#cryptocurrency)
