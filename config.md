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

**All values are required** - except for mail and nodemailer.

```yaml

app :
  url:
    scheme:  e.g. `http` or `https`.
    host:  The host or domain name.
    port:  The local port to run the app on.
  magicNumber:  Token for some private API routes.
  site:  name of your website
  logo:  absolute URL of your logo image

db:
  path:  Path to the LevelDB directory.

currencies:  different currencies accepted by the app
    btc:  default to BTC
      defaultNetwork:  Default bitcoin network for Bitcore. Options are `livenet` or `testnet`.
      networks:  list of networks to be used
        incomingPrivateKey:  HD wallet private key to handle incoming payments.
        outgoingPublicKey:  HD wallet public key to accept outgoing payments.
        documentPrice:  Document certification price in satoshis.
        feeMultiplier:  Multiply estimated fee by this value to change its priority. Defaults to `2`.

mail:
  from:  Name/email to send as.
  to:  Email address to send notifications to.

nodemailer: see [the docs](https://nodemailer.com/about/)
  options:
    service: Node service to send email
    auth:
      user:  GMail account for sending notifications.
      pass:  Gmail password for sending notifications.

```

## Environment variables

In addition, some values may be overridden through environment variables:

* `PORT` - The local port to run the app on.
* `HOST` - The host or domain name
* `HOST_SCHEME` - e.g. `http` or `https`.
* `HOST_PORT` - e.g. `80` or `443`.
* `DB_PATH` - Path to the LevelDB directory.
* `DOCUMENT_PRICE` - Document certification price in satoshis.
* `FEE_MULTIPLIER` - Multiply estimated fee by this value to change its
  priority. Defaults to `2`.
* `BITCOIN_NETWORK` - Default bitcoin network for Bitcore. Options are `livenet` or `testnet`.
* `BITCOIN_HD_PRIVATE_KEY` - HD wallet private key to handle incoming payments.
* `BITCOIN_HD_PUBLIC_KEY` - HD wallet public key to accept outgoing payments.
* `MAGIC_NUMBER` - Token for some private API routes.
* `MAIL_FROM` - Name/email to send as.
* `MAIL_TO` - Email address to send notifications to.
* `GMAIL_USER` - GMail account for sending notifications.
* `GMAIL_PASS` - Gmail password for sending notifications.



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
