## Using Bitcoin Safe

### 1. ❗Bitcoin is NOT anonymous❗(onchain)
I cannot stess this enought. 

## Problem

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

## Solution
1. Use Lightning Network whenever possible
2. Practice Coin Control, label your UTXos
3. Avoid creating a long peel chains
   - Ideally spend whole UTXO with no change at all
   - If possible, use Lightning to fill the missing amount or refund the overpayment
4. **NEVER** consolidate, if you end up with some small UTXOs, **do NOT mixed them with other coins!**
   - rather donate them to some charity
   - or if you are greedy fuck, swap them for Lightning on [fixedfloat.com](https://fixedfloat.com/) or similar

### 2. Use Lightning Network

### 3. Use Lightning Network
