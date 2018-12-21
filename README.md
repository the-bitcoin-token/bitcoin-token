# Bitcoin Token

BitcoinToken is a library that makes it easy to integrate Bitcoin into web applications. It consists of three tools

 * **[BitcoinSource](https://github.com/the-bitcoin-token/BitcoinSource)** a readable JS implementation of Bitcoin
 * **[BitcoinWallet](http://www.bitcointoken.com/docs#bitcoinwallet)** send, store, and receive Bitcoin
 * **[BitcoinDb](http://www.bitcointoken.com/docs#bitcoindb)** store data on the blockchain
 * **[BitcoinToken](http://www.bitcointoken.com/docs#bitcointoken)** issue tokens

More information at [www.bitcointoken.com](http://www.bitcointoken.com) and [www.bitcointoken.com/docs](http://www.bitcointoken.com/docs).

## Install

````terminal
npm i bitcointoken
````

If you only want to use BitcoinWallet you are done. If you want to use BitcoinDb or BitcoinToken as well you need to install and run a non-standard server in a separate terminal window.

### Install and run a non-standard server

We recommend to use <a href="https://www.docker.com/">Docker</a> and <a href="https://docs.docker.com/compose/">Docker Compose</a> to build and run the server using the commands below. If you do not use docker you can find instructions in the <a href="https://github.com/the-bitcoin-token/bitcoin-non-standard-server">Github repo</a>.

````shell
git clone https://github.com/BitcoinDB/bitcoin-non-standard-server.git
cd bitcoin-non-standard-server
docker-compose up
````


## Run in Node

After installing BitcoinToken, create a file <code>index.js</code> and run it with `node index.js`.

````javascript
// index.js
const { Wallet } = require('bitcointoken')

const wallet = new Wallet()
console.log(`address: ${wallet.getAddress()}`)
````


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

Install and run browserify, then open <code>index.html</code> in a web browser.

````shell
npm install -g browserify
browserify index.js > bundle.js
````


## Troubleshooting

Ask in our [Telegram group](https://t.me/joinchat/FMrjOUWRuUkNuIt7zJL8tg) or have a look at the [documentation](http://www.bitcointoken.com/docs).

