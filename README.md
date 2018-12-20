# Bitcoin Token

The BitcoinToken library is a collection of tools that make it easy to integrate Bitcoin into web applications.

 * **BitcoinSource** a readable JS implementation of Bitcoin
 * **BitcoinWallet** send, store, and receive Bitcoin
 * **BitcoinDb** store data on the blockchain
 * **BitcoinToken** issue tokens

The docs are at [www.bitcointoken.com/docs](https://www.bitcointoken.com/docs)


## Install

You need <a href="https://docs.npmjs.com/downloading-and-installing-node-js-and-npm">node.js and npm</a> installed. Then install BitcoinToken using


````terminal
npm i bitcointoken
````

If you only want to use BitcoinWallet you are done. If you want to use BitcoinDb or BitcoinToken you need to install and run a non-standard server in a separate terminal window.

### Install and run a non-standard server

We recommend to use <a href="https://www.docker.com/">Docker</a> and <a href="https://docs.docker.com/compose/">Docker Compose</a> to build and run the server using the commands below. If you do not use docker you can find instructions in the <a href="https://github.com/the-bitcoin-token/bitcoin-non-standard-server">Github repo</a>.

````shell
git clone https://github.com/BitcoinDB/bitcoin-non-standard-server.git
cd bitcoin-non-standard-server
docker-compose up
````



## Run in Node

After installing BitcoinToken, create a file <code>index.js</code>

````javascript
// index.js
const { Wallet } = require('bitcointoken')

const wallet = new Wallet()
console.log(`address: ${wallet.getAddress()}`)
````

Run the code

````shell
node index.js
````

The output should be similar to `address: mwZd1bgRYhxY4JLyx1sdEGBYFZnXVDvgmp`


## Run in the browser

After installing BitcoinToken, create files <code>index.js</code> and <code>index.html</code>.

````javascript
// index.js
const Bitcoin = require('bitcointoken')

const wallet = new Bitcoin.Wallet()
document.body.innerHTML = `address: ${wallet.getAddress()}`
````


````html
<!-- index.html -->
<html>
  <body>
    <script src="./bundle.js"></script>
  </body>
</html>
````

Install and run browserify

````shell
npm install -g browserify
browserify index.js > bundle.js
````

Open <code>index.html</code> in a web browser


## Troubleshooting

If you have a problem that is not discussed here, please let us know in the <a href="https://t.me/joinchat/FMrjOUWRuUkNuIt7zJL8tg">Telegram group</a>.

### "Communication error: Service unavailable"

The problem is most likely that you are not running the non-standard server.

### "Error: Insufficient balance in address ..."

You have to fund your wallet. First check that the same wallet is generated every time you run your code: Run it multiple times and check if the  same address is logged every time.

If so you can fund that address using a [testnet faucet](https://coinfaucet.eu/en/bch-testnet/).

Otherwise you need to initialize your object from a mnemonic. To generate a mnemonic, create the file <code>generate-mnemonic.js</code> and run it using <code> node --experimental-repl-await generate-mnemonic.js</code>. Now you can generate your object using the logged mnemonic using <code>Wallet.fromMnemonic()</code> and fund it using the [testnet faucet](https://coinfaucet.eu/en/bch-testnet/)

````javascript
const { Wallet } = require('bitcointoken')
console.log(Wallet.getRandomMnemonic())
````
