# Configuration


To get started, you need to create a `local` config file with the
desired values for your instance.

Just copy the `sample-local.yaml` to `local-development.yaml` and edit it.

```sh
cp config/sample-local.yaml config/local-development.yaml
```

We use
[node-config](https://github.com/lorenwest/node-config/wiki/Configuration-Files)
to manage config files.

## Config File

```yaml
# config/local-development.yaml

app:
  port:  # The local port to run the app on.
  url:
    scheme:  # e.g. `http` or `https`.
    host:  # The host or domain name.
    port:  # The port of the site (e.g., `80` or `443`)
  magicNumber:  # Token for some private API routes.
  site:  # Name of your website.
  logo:  # Absolute URL of your logo image.
  defaultChain:  # e.g. `btc`
  defaultNetwork:  # e.g. `mainnet` or `testnet`

db:
  path:  # Path to the LevelDB directory.

chains:  # Supported chains
  btc:  # Bitcoin
    networks:  # List of configured networks.
      mainnet:
        incomingPrivateKey:  # HD wallet private key to handle incoming payments.
        outgoingPublicKey:  # HD wallet public key to accept outgoing payments.
        documentPrice:  # Document certification price in satoshis.
        feeMultiplier:  # Multiply estimated fee by this value to change its priority. Defaults to `2`.
      testnet:
        incomingPrivateKey: tprvXXXXX
        outgoingPublicKey: tpubXXXXX
        documentPrice: 100000
        feeMultiplier: 4

mail:
  from:  # Name/email to send as.
  to:  # Email address to send notifications to.

nodemailer:  # see [the docs](https://nodemailer.com/about/)
  options:
    service: # Node service to send email, e.g. `Gmail`
    auth:
      user:  # Account for sending notifications.
      pass:  # Password for sending notifications.
```

## Environment variables

In addition, some values may be overridden through environment variables:

* `PORT` - The local port to run the app on.
* `HOST` - The host or domain name
* `HOST_SCHEME` - e.g. `http` or `https`.
* `HOST_PORT` - e.g. `80` or `443`.
* `DB_PATH` - Path to the LevelDB directory.
* `BITCOIN_CHAIN` - Default bitcoin chain for Bitcore. Options are `btc` or `bch`.
* `BITCOIN_NETWORK` - Default bitcoin network for Bitcore. Options are `livenet` or `testnet`.
* `MAGIC_NUMBER` - Token for some private API routes.
* `MAIL_FROM` - Name/email to send as.
* `MAIL_TO` - Email address to send notifications to.
* `GMAIL_USER` - GMail account for sending notifications.
* `GMAIL_PASS` - Gmail password for sending notifications.

## Dynamic Pricing

In order to enable dynamic pricing, remove the `documentPrice` setting from the
network configuration.

The document price will be based on the current estimated fee, times the fee
multiplier, and rounded up to the nearest 0.25 mBTC.

## About wallets

**Note**: You must create two different HD wallets. The first wallet is the
*Incoming* wallet, which is used to generate payment addresses for the user, and
to sign transactions embedding docproofs. The second wallet is the *Outgoing*
wallet, which is used to generate addresses where the change from docproof
transactions is sent.

The private key for the *Incoming* wallet must be the `BITCOIN_HD_PRIVATE_KEY`.

The public key for the *Outoing* wallet must be `BITCOIN_HD_PUBLIC_KEY`. The
private key for the change wallet should be kept secret, and can be used later to sweep the funds.

See the [First Proof Tutorial](./first-proof.md) to learn more about wallets.

# Insight API server

In order to interact with the Bitcoin network, the software has to connect to a
[Bitcore Node] server running these services:

* [insight-api]
* [specialops]

Please consult the documentation on the GitHub repos for how to install and run
bitcore-node with these services.

The URLs for the Insight server can be changed in your local config file using
the following settings:


```yaml
services:
  insight:
    btc:
      mainnet:
        url: https://insight.example.com
        api: /api
      testnet:
        url: https://test-insight.example.com
        api: /api
```

The `api` path must match the configuration `routePrefix` setting for your
[insight-api] service.

[Bitcore Node]: https://github.com/bitpay/bitcore-node
[insight-api]: https://github.com/bitpay/insight-api
[specialops]: https://github.com/poexio/specialops/