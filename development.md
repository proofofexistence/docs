# Development


### Building

```sh
npm run build
```

### Testing

Run the tests with:

```sh
npm test
```

### Development

You must use a **Testnet3 wallet** for the public and private keys. It is
recommended to change the database path to a `/tmp` location.

Run the app in dev mode with:

```sh
npm run watch
```

### Production

Create a `config/local-production.yaml` file with your configuration.

Build the app:

```sh
NODE_ENV=production npm run build
```

The production app is run with:

```sh
NODE_ENV=production npm run serve
```

To clean up:

```sh
npm run clean
```

#### Test Certifications

Finally, to certify a document on the testnet blockchain:

1. Create a `config/local-development.yaml` file with all your configuration
1. Set up a new testnet wallet and note down the Private Key WIF
1. Fund the testnet wallet, e.g. using a Bitcoin testnet3 faucet
1. Run the app locally in dev mode, with ngrok active
1. Submit a document hash to the running app on localhost
1. Note down the target address for payment
1. Send payment with `node scripts/payment.js PRIVATE_KEY_WIF TARGET_ADDRESS`
1. Wait for the transaction to be confirmed on testnet blockchain


### Release a new version

Interactive CLI tool to release versions

```
npm run release
```
