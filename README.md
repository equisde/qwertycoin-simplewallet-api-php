# PHP class for interaction with Qwertycoin Simplewallet API

Simple php class for interaction with [Qwertycoin Simplewallet JSON RPC API](https://github.com/qwertycoin-org/qwertycoin-api-php).

## Available methods ##
* getBalance
* getAddress
* getHeight
* getTransfers
* getPayments
* transfer
* store
* reset
* genPaymentId
* checkAddress
* checkPaymentId

### getBalance ###

Return wallet balance.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$balance = $qwcWallet->getBalance();
?>
```
###### Output data: ######
```
{
	"status": true,
	"available_balance": 100,
	"locked_amount": 0
}
```

### getAddress ###

Returns wallet address.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$balance = $qwcWallet->getAddress();
?>
```
###### Output data: ######
```
{
	"status": true,
	"address": "KctbUcra6R4DLHmDYbVvfrDBLQHxpjN2xVKs9EcmsDivXVWiu15uuF2YKgZfMPEYgT5dS4JGuJERmY46AwEVehwq35acL8f "
}
```

### getHeight ###

Returns the last top known block height for simplewallet. This method can be used to verify that simplewallet is correctly synchronized.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$balance = $qwcWallet->getHeight();
?>
```
###### Output data: ######
```
{
	"status": true,
	"height": 201631
}
```

### getTransfers ###

Returns the list of all the wallet's incoming and outgoing transfers.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$balance = $qwcWallet->getTransfers();
?>
```
###### Output data: ######
```
{
	"status": true,
	"transfers": [{
		"address": "",
		"amount": 1,
		"fee": 0.01,
		"blockIndex": 20561,
		"output": false,
		"paymentId": "1e9abacae3c48f12ffa1cc7ded3adc0b001e194379276585be7b8e06d2830456",
		"time": 1518824088,
		"transactionHash": "097ddca8c0e788ddfbe677e5fa12a887de1df0d4179a31e9f0c1f9d1deb02810",
		"unlock_time": 0
	}, {
		"address": "QWC1L4aAh5i7cbB813RQpsKP6pHXT2ymrbQCwQnQ3DC4QiyuhBUZw8dhAaFp8wH1Do6J9Lmim6ePv1SYFYs97yNV2xvSbTGc7s",
		"amount": 1,
		"fee": 0.01,
		"blockIndex": 21589,
		"output": true,
		"paymentId": "4e0f2754ea0622150df530c96c900c185e816ef8436f50b0753baf0848a0d6aa",
		"time": 1519096397,
		"transactionHash": "575efc958492880d50b14756fb2de66e4c195a59a555915d75a74106b6275496",
		"unlock_time": 0
	}]
}
```

### getPayments ###

Receives all the payments with a corresponding payment_id that were sent to the wallet. This method is used to get the QWC payments for the 3rd party services. As Qwertycoin uses only one address to receive QWC deposits, a unique payment_id should be assigned and shown to each user. The method will return all the payments for this user.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$paymentId = "1e9abacae3c48f12ffa1cc7ded3adc0b001e194379276585be7b8e06d2830456";
	$qwcWallet->getPayments($paymentId);
?>
```
###### Output data: ######
```
{
	"status": true,
	"payments": [{
		"amount": 1,
		"block_height": 200561,
		"tx_hash": "5d67329ce94e8a127b1490b281feb13f74c18d0ac1dbe49338c1663c34a27738",
		"unlock_time": 0
	}]
}
```

### transfer ###

Transfer QWC to several destinations with specified fee, mixin ambiguity degree, and unlock time.

Please note: fee param is a mandatory and should not be less than 0.01 QWC.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$paymentID = $qwcWallet->genPaymentId();
	$fee = $qwcWallet->genPaymentId();
	$unlock_time = 0;
	$transData = [
		[
			"amount" => "100",
			"address" => "QWC1L4aAh5i7cbB813RQpsKP6pHXT2ymrbQCwQnQ3DC4QiyuhBUZw8dhAaFp8wH1Do6J9Lmim6ePv1SYFYs97yNV2xvSbTGc7s"
		]
	];
	$qwcWallet->transfer($transData, $paymentID, $fee, $unlock_time);
?>
```
###### Output data: ######
```
{
	"status": true,
	"payments": [{
		"amount": 1,
		"tx_hash": "5d67329ce94e8a127b1490b281feb13f74c18d0ac1dbe49338c1663c34a27738",
		"payment_id": "4e0f2754ea0622150df530c96c900c185e816ef8436f50b0753baf0848a0d6aa"
	}]
}
```

### store ###

Store wallet data.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$balance = $qwcWallet->store();
?>
```
###### Output data: ######
```
{
	"status": true
}
```

### reset ###

Erases simplewallet's internal state but keeps safe the wallet.bin. The method should be used to re-synchronize the wallet from scratch. The next refresh (which is automatically called each 20 seconds) will update the simplewallet state.

###### Example of usage: ######
```
<?php
	$qwcWallet = new Qwerty();
	$balance = $qwcWallet->reset();
?>
```
###### Output data: ######
```
{
	"status": true
}
```

### genPaymentId ###

Generate payment id

###### Example of usage: ######
```
<?php
	Qwerty::genPaymentId();
?>
```
###### Output data: ######
```
4e0f2754ea0622150df530c96c900c185e816ef8436f50b0753baf0848a0d6aa
```

### checkAddress ###

Check if address valid

###### Example of usage: ######
```
<?php
	Qwerty::checkAddress("QWC1L4aAh5i7cbB813RQpsKP6pHXT2ymrbQCwQnQ3DC4QiyuhBUZw8dhAaFp8wH1Do6J9Lmim6ePv1SYFYs97yNV2xvSbTGc7s");
?>
```
###### Output data: ######
```
true
```

### checkPaymentId ###

Check if payment id valid

###### Example of usage: ######
```
<?php
	Qwerty::checkPaymentId("4e0f2754ea0622150df530c96c900c185e816ef8436f50b0753baf0848a0d6aa");
?>
```
###### Output data: ######
```
true
```

Based on [Lastick/karbo-api-php](https://github.com/seredat/karbowanec/wiki/Simplewallet-JSON-RPC-API) and [Simplewallet JSON RPC API wiki page](https://github.com/seredat/karbowanec/wiki/Simplewallet-JSON-RPC-API)