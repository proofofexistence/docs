# Developer API

This API allows developers to integrate their systems with
proofofexistence's functionality. You can access all of the app's
features through an HTTP interface.

It consists of the following two POST endpoints:

-   `/api/v1/register`: used to register a new
    document's [SHA256 digest](#sha256). Returns a payment address
    where you need to send the bitcoins to have the document certified
    in the blockchain, and the amount of bitcoins you need to send
    expressed in satoshis (100000000 satoshis = 1 BTC)
-   `/api/v1/status`: Receives a digest and returns the
    status of that document. If the status is `pending`, you'll also
    get the payment address and price to confirm the document in the
    blockchain.

As well as a GET endpoint if [specialops](https://github.com/poexio/specialops) service was enable:

-   `/api/v1/docproofs/:hash`: used to search the blockchain for
    all transactions that embed a docproof.

Let's explain the API using an example:

First we compute the SHA256 checksum hexdigest for a file we want to
certify on the blockchain:

    shasum -a 256 somefile.txt
    15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832  somefile.txt

Checking the status for a digest which was never submitted will give
`"success": false`, as follows:

    curl -d d=15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832 https://proofofexistence.com/api/v1/status; echo
    {
      "success": false,
      "reason": "nonexistent"
    }

So, first, we want to register the document's digest in
proofofexistence. This will return a bitcoin payment address, and the
minimum price (in satoshi, 0.00000001 BTC = 1 satoshi) to be paid to
that address in order to get the document proof inserted in the
blockchain.

    curl -d d=15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832 https://proofofexistence.com/api/v1/register; echo
    {
      "success":"true",
      "digest":"15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832",
      "pay_address":"1Zmxnd5CmLqhVnCbEcvxNxCoeqa2qhun3",
      "price":{{ documentPrice }}
    }

If we now check the document's digest status, we get it's pending.

    curl -d d=15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832 https://proofofexistence.com/api/v1/status; echo
    {
      "digest":"15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832",
      "payment_address":"1Zmxnd5CmLqhVnCbEcvxNxCoeqa2qhun3",
      "pending":true,
      "timestamp":"2015-09-28 18:16:50",
      "network":"{{ defaultNetwork }}",
      "success":true,
      "txstamp":"",
      "blockstamp":"",
      "price":{{ documentPrice }}
    }

After making payment to indicated address
(1Zmxnd5CmLqhVnCbEcvxNxCoeqa2qhun3) for a minimum value of {{
documentPrice }} satoshis:

    curl -d d=15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832 https://proofofexistence.com/api/v1/status; echo
    {
      "digest":"15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832",
      "payment_address":"1Zmxnd5CmLqhVnCbEcvxNxCoeqa2qhun3",
      "pending":false,
      "timestamp":"2015-09-28 18:16:50",
      "tx":"f8db93646769eaf614cf5f26fb1bf1b78ee3f83ba6bebb5f7da9223f0022577d",
      "txstamp":"2015-09-28 18:26:31",
      "network":"{{ defaultNetwork }}",
      "success":true,
      "blockstamp":"",
      "price":{{ documentPrice }}
    }

This indicates the address received payment and that the transaction
proving the document's existence was sent to the blockchain. You can
see the document's `timestamp` (when it was received by our servers).
Additional interesting fields are `txstamp` (when payment transaction
was received and certifying transaction with OP\_RETURN was sent to the
blockchain), `tx` (transaction containing proof of existence of the
document), and `blockstamp` (timestamp of the bitcoin block containing
the transaction), which is currently empty because the transaction was
not yet confirmed. Once the certifying transaction is confirmed, you'll
see that field populated:

```sh

    curl -d d=15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832 https://proofofexistence.com/api/v1/status; echo
    {
      "digest":"15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832",
      "payment_address":"1Zmxnd5CmLqhVnCbEcvxNxCoeqa2qhun3",
      "pending":false,
      "timestamp":"2015-09-28 18:16:50",
      "tx":"f8db93646769eaf614cf5f26fb1bf1b78ee3f83ba6bebb5f7da9223f0022577d",
      "txstamp":"2015-09-28 18:26:31",
      "network":"{{ defaultNetwork }}",
      "success":true,
      "blockstamp":"2015-09-28 19:32:54",
      "price":{{ documentPrice }}
    }
```

Finally, you check search for all Bitcoin transactions that embed the
document hash and confirm that your transaction is among them:

```sh
curl https://proofofexistence.com/api/v1/docproofs/15db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832

{
  "pagination": {},
  "items": [
    {
      "blockhash": "000000000000000000be98e21e83c88f791a3fcaf9be217aab127c4a8e7a9341",
      "blockheight": 376553,
      "blocktime": 1443468774,
      "confirmations": 146397,
      "metadata": "444f4350524f4f4615db6dbff590000ea13246e1c166802b690663c4e0635bfca78049d5a8762832",
      "txid": "f8db93646769eaf614cf5f26fb1bf1b78ee3f83ba6bebb5f7da9223f0022577d",
      "outputIndex": 0
    }
  ]
}
```

That's it! If you have any questions/feedback/problems, [drop us an
email](mailto:admin@proofofexistence.com) or [open an issue on Github](https://github.com/proofofexistence/proofofexistence/issues)

### Calculating the SHA256 digest

Here are ways in which you can calculate your document's SHA256 digest.
Of course you can also implement your own, these are here for reference.

-   **Using a client-side JS library**: [crypto.js](/js/crypto.js).
    Usage Example:

```js
  var data = "Hello world!";
  var progress = function(p) {
    var w = (p*100);
    alert(w + "% hashed");
  }

  var crypto_finish = function(hash) {
    alert("Document hash: "+hash);
  }
  
  CryptoJS.SHA256(data,progress,crypto_finish);

```
