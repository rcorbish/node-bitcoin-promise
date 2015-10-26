# node-bitcoin-promise

#Summary
Add Promise support to <A href="//github.com/freewil/node-bitcoin/blob/master/Readme.md">bitcoin</A> package.
It is backward compatible with the original package. If no callback function is
passed in to a command a Promise is returned

#Installation

	npm install bitcoin-promise


#Example
	var bitcoin = require( 'bitcoin-promise' ) ;

	var client = new bitcoin.Client({
		host: 'rcorbish.ydns.eu',
		port: 18332,
		user: 'bitcoinrpc',
		pass: 'password',
		timeout: 30000
	});

	// get a new address and return details about it
	client.getNewAddress()
	.then( function(addr){
		return client.validateAddress( addr ) ;
	}) 
	.then( function(addrInfo){
		console.log( addrInfo ) ;
	}) 
	.catch( function(err){
		console.log( err ) ;
	}) ;


