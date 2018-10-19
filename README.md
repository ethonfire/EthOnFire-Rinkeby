# EthOnFire-Rinkeby
Welcome to Eth OnFire. Eth OnFire is a coin burning contract. The OnFire team took inspiration from the [Bonfire dapp](https://github.com/BonfireEth/Bonfire-15-I) contracts. By burning over half of the coins we can bring value to the entire market not just users of the game. The ETH OnFire contract is made to burn coins but 30% of the ETH from each round is gifted to one randomly chosen user. The winner of the 30% gift is picked using oraclize's random number contract combined with a salt number hidden in a hash submitted before each round. 

# How it Works
1. After the salt number is submitted to the contract by way of hash and the ticket price is set, tickets are open for sale. 

2. After all tickets are sold, a random number must be requested. The random number is requested by any user using the button on the website ("Request Random") or by command line (see "cli" below). The user that successfully requests the random number is gifted the value of one ticket after the coin burn. Their gift can be withdrawn from the contract after the coin burn. Note: there is a 30 minute cool down for the request button/function call.

3. After the random number is received the salt number is revealed by the OnFire team. The salt number modifies the random number to generate the winning ticket number.

4. The last step is to deposit the gifts and burn the coins. This is done by a user (any user) pressing the burn button or calling the burn function using the cli. Note: this function has a 30 minute cool down. 30% of the pot goes to the winning ticket holder, the value of one ticket goes to each of the function callers (Request Random and Burn functions), 1% goes to the OnFire team, and about 69% (less the price of two tickets) is **Burned**.


```javascript

var abi = [{"constant":true,"inputs":[],"name":"senderHolderAddress","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"precommitHash","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"burnWallet","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"prizeAddress","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"reward","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"pastWinnerCounter","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"messageOut","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"ticketCounter","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"value","type":"uint256"}],"name":"setPrice","outputs":[],"payable":true,"stateMutability":"payable","type":"function"},{"constant":true,"inputs":[],"name":"stage3","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"price","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"oracleRandomNumber","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"secretHash","type":"bytes32"}],"name":"submitSecretHash","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[],"name":"incinerate","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"stage4","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"uint256"}],"name":"pastWinners","outputs":[{"name":"incinerationTime","type":"uint256"},{"name":"winnersAddress","type":"address"},{"name":"prizeAmount","type":"uint256"},{"name":"preHash","type":"bytes32"},{"name":"postTxtOne","type":"string"},{"name":"secretNumber","type":"uint256"},{"name":"postTxtTwo","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"saltNum","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"stage6","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"ignite","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"anonymous":false,"inputs":[{"indexed":false,"name":"burned","type":"uint256"}],"name":"IncinerationComplete","type":"event"},{"payable":true,"stateMutability":"payable","type":"fallback"},{"constant":false,"inputs":[],"name":"withdraw","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"anonymous":false,"inputs":[{"indexed":false,"name":"txt","type":"string"}],"name":"Reader","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"name":"txt","type":"string"},{"indexed":false,"name":"secretHash","type":"bytes32"}],"name":"SaltHashSet","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"name":"val","type":"uint256"}],"name":"NewPrice","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"name":"ticketsLeft","type":"uint256"}],"name":"TicketSold","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"name":"secretHash","type":"bytes32"},{"indexed":false,"name":"textOne","type":"string"},{"indexed":false,"name":"secretNum","type":"uint256"},{"indexed":false,"name":"textTwo","type":"string"}],"name":"RevealedHash","type":"event"}]
var foo = web3.eth.contract(abi)
var contract = firePlace.at('0x289f9bee36a3fb871915ef26d0cac00f77ecdb34')


var bar = contract.RevealedHash({}, {fromBlock: 0, toBlock: 'latest'});

bar.watch(function(error, result){console.log(result.args.secretHash + " " + result.args.textOne + " " + result.args.secretNum + " " + result.args.textTwo)});


var baz = contract.GiftDeposited({}, {fromBlock: 0, toBlock: 'latest'});

baz.watch(function(error, result){console.log(result.args.giftRecipient + " " + result.args.amount)});

```
##### Request Random Number
```
contract.ignite({from: your.address, gas: 1000000})
```
##### Burn Request
```
contract.incinerate({from: your.address, gas: 1000000})
```

# More Info
1. The OnFire team submits a salt number by way of a hash. The salt number modifies the random number from oraclize to protect against the possibility of a bad actor on the random number generation contract. The salt number is submitted before a round is opened and cannot be changed, because in order to reveal the salt number the hashes must match. 

2. The selfdestruct function burns all of the ETH held in the contract without benefit to the OnFire team.

# Mainnet
[Eth OnFire](https://ethonfire.github.io/EthOnFire/)

##### Credits
* Bootstrap timeline by [siddharth4753](https://bootsnipp.com/snippets/Q0ppE) 
* JS and game design [Bonfire](https://github.com/BonfireEth/Bonfire-15-I)
