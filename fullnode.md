## Decred Full Node Script - Super Fast!

Here's how you can run a Decred full node with just one command on a Virtual Private Server or something like a Raspberry Pi or other home server solution.

If you already have a VPS and don’t want to follow the tutorial, just run this curl command and your Decred full node will be good to go.

`curl -L https://node.dcr.pw | bash`

Raspberry Pi:

`curl -L https://node.dcr.pw/pi | bash`

### Why should you run a full node?

Decred relies on having a peer-to-peer network of nodes that fully validate all transactions and blocks and then relays them to other full nodes. Running a full node contributes to the overall security of the network, and helps ensure there are nodes available to serve lightweight SPV wallets, which are used in core applications like Decrediton, Bison Relay, and the Decred DEX.

### Requirements

What’s great about Decred’s node is it’s only about 12 gigabytes in size so you don't need much hard drive space. So for this demonstration, I’m going to pick a cheap VPS option. You’ll want at least 2 gigabytes of ram and 40 gigabytes of harddrive space because the node will grow in size over time.

### OS - Ubuntu or Debian

I’ll choose Ubuntu as my OS. (22.04 64 bit)

### SSH using Putty

And once its set up and I’m in the control panel, I’ll take the IP address and password they give me and securely connect to my VPS using an application called [PUTTY](https://putty.org/).

### Update The Server

Now I’ll make sure the server is up to date by running `sudo apt update && sudo apt upgrade` After that’s done I’ll type `reboot` to reboot the server.

Log back in again using PUTTY. And now I’ll run our handy script which is going to set everything up.

### Running the Curl Command

Ubuntu/Debian:

`curl -L https://node.dcr.pw | bash`

Raspberry Pi:

`curl -L https://node.dcr.pw/pi | bash`

And that’s it. Our node is going to spend the next hour or so syncing to the latest block, and from there it will do its job of validating transactions and helping the network.

### Check Status

I can run the command `decred.sh` at any time to see what the node is doing.

I can also run the command `htop` to view memory and CPU usage. Press control and x to exit this view.

View the source code of this curl command on [Github](https://github.com/jzbz/dcr-node/blob/master/index.html). Shout out to Decred Jesus for putting this all together.

What’s great is it runs as a service, so the node will automatically start up whenever the server is running.

My node is now part of a global network and is visible on the [Decred mapper website](https://nodes.jholdstock.uk/). (Note this might take a few days to appear)

And that’s all! Thanks for reading, and I hope you consider running a Decred full node.
