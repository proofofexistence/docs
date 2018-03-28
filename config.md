# Configuration

Edit `.env` for environment variables. All values are **required**.

* `PORT` - The local port to run the app on.
* `HOST` - The host or domain name. (`NOTE: you maybe need to use ngrok`)
* `HOST_SCHEME` - e.g. `http` or `https`.
* `HOST_PORT` - e.g. `80` or `443`.
* `DB_PATH` - Path to the LevelDB directory.
* `DOCUMENT_PRICE` - Document certification price in satoshis.
* `FEE_MULTIPLIER` - Multiply estimated fee by this value to change its
  priority. Defaults to `2`.
* `BLOCKCYPHER_TOKEN` - BlockCypher API token, Register it [here](https://www.blockcypher.com/).
* `BITCOIN_NETWORK` - Default bitcoin network for Bitcore. Options are `livenet`
  or `testnet`.
* `BITCOIN_HD_PRIVATE_KEY` - HD wallet private key to handle incoming payments.
* `BITCOIN_HD_PUBLIC_KEY` - HD wallet public key to accept outgoing payments.
* `MAGIC_NUMBER` - Token for some private API routes.
* `MAIL_FROM` - Name/email to send as.
* `MAIL_TO` - Email address to send notifications to.
* `GMAIL_USER` - GMail account for sending notifications.
* `GMAIL_PASS` - Gmail password for sending notifications.

Any environment variables that are set when the app is run will override the
values in the `.env` file.

**Note**: You must create two different HD wallets. The first wallet is the
*Incoming* wallet, which is used to generate payment addresses for the user, and
to sign transactions embedding docproofs. The second wallet is the *Outgoing*
wallet, which is used to generate addresses where the change from docproof
transactions is sent.

The private key for the *Incoming* wallet must be the `BITCOIN_HD_PRIVATE_KEY`.

The public key for the *Outoing* wallet must be `BITCOIN_HD_PUBLIC_KEY`. The
private key for the change wallet should be kept secret, and can be used later
to sweep the funds.
