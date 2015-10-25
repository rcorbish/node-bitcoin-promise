# node-bitcoin-promise
Add Promise support to <A href="//github.com/freewil/node-bitcoin/blob/master/Readme.md">bitcoin</A> package 


This is fully backward compatible with the original package. If no callback function is
passed in to a command a Promise is returned


	var bitcoin = require( 'bitcoin-promise' ) ;

	var client = new bitcoin.Client({
		host: 'rcorbish.ydns.eu',
		port: 18332,
		user: 'bitcoinrpc',
		pass: 'password',
		timeout: 30000
	});

	var promiseAddr1 = client.dumpPrivKey( addr2 ) ;
	var promiseAddr2 = client.dumpPrivKey( addr3 ) ; 
	
	var target = {} ;		// where will the funds be sent ...
	target[receiver] = 1 - fee ;	// how much am I asking for ?
	var promiseRawTx  = client.createRawTransaction( [ { txid: txid, vout:vout,
						scriptPubKey:scriptPubKey,
						redeemScript:script}],
						target ) ;
	
	Promise.all( [ promiseRawTx, promiseAddr1, promiseAddr2 ] )
	.then(function( data ) {
		var rawTx = data[0] ;
		return client.signRawTransaction( rawTx, [ { txid: txid, vout:0,
			scriptPubKey:scriptPubKey,
			redeemScript:script}], [data[1], data[2] ] ) ;
	} )
	.then(function( signedTx ) {
		return client.sendRawTransaction( signedTx.hex ) ;
	} )
	.then(function( data ) {
		console.log( "Sent TXID:", data ) ;
	} )
	.catch(function(err) {
		console.log( "E R R O R", err ) ;
	} ) ;
