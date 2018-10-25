# Bitcoin Token

BitcoinToken is a toolkit for building blockchain enabled web applications on top of Bitcoin Cash. It consists of three tools: a wallet, a database built on the blockchain, and a token solution. These are meant to be combined by the developer to adapt to the desired application. BitcoinToken is written in Javascript and runs in the browser and in node.

## Install

To install BitcoinToken run

```
npm i bitcointoken
```

To test the library, you also need to install and run a [Bitcoin non-standard server](https://github.com/the-bitcoin-token/bitcoin-non-standard-server).

You can find more information and developer documentation at <a href="http://www.bitcointoken.com">www.bitcointoken.com</a> and <a href="http://www.bitcointoken.com/docs">www.bitcointoken.com/docs</a>.

## Example usage

You can issue a token in Javascript. You can find more examples in the <a href="http://www.bitcointoken.com/docs">BitcoinToken docs</a>.

```
const token = new BitcoinToken()
await token.create({ balance: '10' })
await token.getBalance() 
await token.send(1, <public key>)
```