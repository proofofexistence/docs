# Get Started

You can run your own instance by following these steps

### Setup

To use Proof of Existence, you need to have [Node JS](https://nodejs.org/en/) installed.  
We recommend using the 8.11.1 version (LTS).  

#### Install Node

You can download `node` from the official [Node JS website](https://nodejs.org/en/).

For Mac OS X, you can also use [`brew`](http://brew.sh) - follow [this guide](https://treehouse.github.io/installation-guides/mac/node-mac.html).  
To install a specific version, you can use [`nvm`](https://github.com/creationix/nvm) that will help you manage different installations of node.

### Installation

Just download the code from Github and install the dependencies.

```sh
git clone git@github.com:proofofexistence/proofofexistence.git
cd proofofexistence
npm install
```

Now let's build the CSS assets before starting the app. 

```sh
npm run build
```

#### Configuration

You need to create a config file to store your own server, BTC wallet and email information.

```sh
cp config/test.yaml config/local-development.yaml
```

Report to the [config page](config.md) to learn more about config variables.

### Running

```sh
npm start
```

The app will be listening at http://localhost:3004/.
