# Introduction
~~This is a living document I maintain for common ops I want to do with my raspberry pi.~~
_I've stopped staking `QTUM` because I want to do something else now. I'm not maintaining this document anymore but I'll leave it here. You probably will just need to change the version strings_

## Pre Requisite
Hopefully you have gone through [setting up Qtum Staking on your Rpi](https://steemit.com/qtum/@cryptominder/qtum-staking-tutorial-using-qtumd-on-a-raspberry-pi-3)

### Useful Alias
I found that the following alias does come in handy:-

```bash
alias qtum-cli='~/qtum-wallet/bin/qtum-cli'
```

## Updating the Rpi
_I'm assuming that the alias for `qtum-cli` was set. Otherwise, use the full path._


Download the [latest version of Qtum core](https://github.com/qtumproject/qtum/releases).

```bash
# NOTE: use the correct link!! The following link is for v0.14.16
pi@raspberrypi:~ $ wget https://github.com/qtumproject/qtum/releases/download/mainnet-ignition-v0.14.16/qtum-0.14.16-arm-linux-gnueabihf.tar.gz
```


Stop `qtumd`

```bash
sudo systemctl stop qtumd.service
```


Extract the downloaded tarball into the old folder (overwriting the current files).
```bash
pi@raspberrypi:~ $ tar --strip 1 -C ~/qtum-wallet/ -xf ~/qtum-0.14.16-arm-linux-gnueabihf.tar.gz
```

Start the `qtumd` service
```bash
pi@raspberrypi:~ $ sudo systemctl start qtumd
```


You can check the version using:
```bash
pi@raspberrypi:~ $ qtum-cli --version
Qtum Core RPC client version v0.14.16.0
```


Staking should've been stopped by now.
```bash
pi@raspberrypi:~ $ qtum-cli getstakinginfo
{
  "enabled": true,
  "staking": false,
  "errors": "",
  "currentblocksize": 0,
  "currentblocktx": 0,
  "pooledtx": 0,
  "difficulty": 3600038.147027622,
  "search-interval": 0,
  "weight": 4274282551,
  "netstakeweight": 1611432403770830,
  "expectedtime": 0
}
```

You will need to use the `getwalletinfo` subcommand to reset the `unlocked_until` field to a large value.
```bash
pi@raspberrypi:~ $ qtum-cli -stdin walletpassphrase
YOUR PASSPHRASE HERE
9999999999
true
<CTRL-D>
```

Now, we should be staking again!! 
```bash
pi@raspberrypi:~ $ qtum-cli getstakinginfo
{
  "enabled": true,
  "staking": true,
  "errors": "",
  "currentblocksize": 1000,
  "currentblocktx": 0,
  "pooledtx": 16,
  "difficulty": 5709822.553567163,
  "search-interval": 2896,
  "weight": 4274282551,
  "netstakeweight": 1679593176630296,
  "expectedtime": 50298014
}
```

