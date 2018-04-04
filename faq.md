# FAQ

### How can I get my `BITCOIN_HD_PRIVATE_KEY` and `BITCOIN_HD_PUBLIC_KEY`?

For now, the [bcwallet](https://github.com/blockcypher/bcwallet) is the best choice.

When you start the wallet, choose the type of network, then you should get all of these.

### It always show 'Waiting for payment' even I send the payment in my local node

Because the core code use webhooks from BlockCypher, please check the status of webhooks

See [List WebHooks
Endpoint](https://www.blockcypher.com/dev/bitcoin/#using-webhooks). It usually
happens with incorrect `host` or `port` setting in the config file.

### How do I extract funds from the app?

To extract the funds from the app you should:

1. From the project's root directory, run:

  ```sh
  curl -X POST https://proofofexistence/sweep/$MAGIC_NUMBER > /tmp/proofofexistence-paths.txt
  ```

2. Run

  ```
  npm run sweep
  ```

When prompted, enter these:

* hdPrivateKey - the master extended private key that is paired with the
  `outgoingPublicKey` from the config file
* file - the `/tmp/proofofexistence-paths.txt` file generated in step 1
* address - bitcoin address where you want to send funds to
