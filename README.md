# Reddit ALTcointip bot

## Introduction

For introduction to and use of LITEDOGE ALTcointip bot, see __http://www.reddit.com/r/ALTcointip/wiki/index__

## Getting Started

>**NEW: See [altcointip-chef](https://github.com/vindimy/altcointip-chef) for Chef-automated installation and configuration of ALTcointip bot.**

### Python Dependencies

The following Python libraries are necessary to run ALTcointip bot:

* __jinja2__ (http://jinja.pocoo.org/)
* __pifkoin__ (https://github.com/dpifke/pifkoin)
* __praw__ (https://github.com/praw-dev/praw)
* __sqlalchemy__ (http://www.sqlalchemy.org/)
* __yaml__ (http://pyyaml.org/wiki/PyYAML)

### Install in Ubuntu / Debian
 `sudo apt-get install python-setuptools python-dev markdown python-pip python-all debhelper python-mysqldb`
   
   `sudo pip install jinja2 praw sqlalchemy pyyaml stdeb`



### Database
  `sudo apt-get install mysql-server php5-mysql`
  
  `sudo mysql_install_db`
  
  `sudo mysql_secure_installation`
  
  You will be asked to enter the password you set for the MySQL root account. Next, it will ask you if you want to change that password. If you are happy with your current password, type “n” for “no” at the prompt.
  
  
  For the rest of the questions, you should simply hit the “ENTER” key through each prompt to accept the default values. This will remove some sample users and databases, disable remote root logins, and load these new rules so that MySQL immediately respects the changes we have made.
  
  
Create a new MySQL database instance and run included SQL file [altcointip.sql](altcointip.sql) to create necessary tables. Create a MySQL user and grant it all privileges on the database. If you don't like to deal with command-line MySQL, use `phpMyAdmin`.

### Coin Daemons

###litedoged install from source

`apt-get update`
`apt-get install nano build-essential libssl-dev libdb++-dev libboost-all-dev libminiupnpc-dev git`
`git clone https://bitbucket.org/ctgiant/litedoge.git`
`cd litedoge/src`
`apt-get update`
`make -f makefile.unix`
`apt-get update`
`cp litedoged /bin`
`litedoged` (will error but needed to create files)
`cd ~`
`cd .litedoge`
`nano litedoge.conf`
###Add Lines
`server=1`
`enableaccounts=1`
`allowip=127.0.0.1`
`maxconnections=1024`
`staking=0`
`rpcport=18340`
`rpcuser=litedogerpc`
`rpcpassword=xxSECURExx34334xxMExxc8gR3P7Pfsds45DFD8VRLbQqTgD13`
`daemon=1`

###Encrypt Wallet
litedoged encryptwallet passwordyouwant-REMEMBER-THIS

It will take some time for the daemon to download the blockchain, after which you should verify that it's accepting commands (such as `litedoged getinfo` and `litedoged listaccounts`).

###clone altcointip
 `cd`
 `git clone http://github.com/dogedart/altcointip`
 
 ###install pifkoin
`cd`
`git clone https://github.com/dpifke/pifkoin`
`cd pifkoin`
`make`
`sudo make install`
`cd`
`ln -s pifkoin/python altcointip/src/ctb/pifkoin`

###setup altcointip
`cd ~/altcointip`
`mysql -u root -p`

You will be prompted for a password, enter the one you entered when we setup mysql:
   `CREATE DATABASE tipbot;`
   `use tipbot;`
   `source altcointip.sql;`
   `exit`
   
   Now we have everything setup for the bot to run! Now it's time to customize the bot, so we have to enter the conf files.
  `cd src`
   `cp conf-sample conf -r`
   `cd conf`
   
   First, let's edit the username and password the bot uses to communicate with the mysql server.
   `nano db.yml`
   
   This will open up a text editor. You need to edit the third line and enter in your mysql password we've been using a few times in this tutorial between the quotation marks. When you're done, hit control+x and press y to save your changes. Now lets edit your reddit settings.
   `nano reddit.yml`
   
   
   First change 'mybotuser' and 'mybotpass' to your bots username and password, respectively. Then, on line 16, change YOUR_NAME to your reddit account (not the bot's) so that users can contact you if they have any issues. On line 38, change 'mybotuser' to your bots username. Now close and save your file, and we will move on to our last configuration file.
  `nano regex.yml`
   
   On line 9, change both instances of mybotuser to your bot's name. Your bot is now ready to run! Type:
   `cd altcointip/src`
Now, to run the bot type:
   `sh _start.sh`
   
### Reddit Account

You should create a dedicated Reddit account for your bot. Initially, Reddit will ask for CAPTCHA input when bot posts a comment or message. To remove CAPTCHA requirement, the bot account needs to accumulate positive karma, Reddit Gold is also required.

### Configuration

Copy included set of configuration files [src/conf-sample/](src/conf-sample/) as `src/conf/` and edit `reddit.yml`, `db.yml`, `coins.yml`, and `regex.yml`, specifying necessary settings.

Most configuration options are described inline in provided sample configuration files.

### Running the Bot

1. Ensure MySQL is running and accepting connections given configured username/password
1. Ensure each configured coin daemon is running and responding to commands
1. Ensure Reddit authenticates configured user. _Note that from new users Reddit will require CAPTCHA responses when posting and sending messages. You will be able to type in CAPTCHA responses when required._
1. Execute `_start.sh` from [src](src/) directory. The command will not return for as long as the bot is running.

Here's the first few lines of DEBUG-level console output during successful initialization.

    user@host:/opt/altcointip/altcointip/src$ ./_start.sh
    INFO:cointipbot:CointipBot::init_logging(): -------------------- logging initialized --------------------
    DEBUG:cointipbot:CointipBot::connect_db(): connecting to database...
    INFO:cointipbot:CointipBot::connect_db(): connected to database altcointip as altcointip
    DEBUG:cointipbot:CtbCoin::__init__(): connecting to Peercoin...
    DEBUG:bitcoin:Read 5 parameters from /opt/altcointip/coins/ppcoin/ppcoin.conf
    DEBUG:bitcoin:Making HTTP connection to 127.0.0.1:19902
    INFO:cointipbot:CtbCoin::__init__():: connected to Peercoin
    INFO:cointipbot:Setting tx fee of 0.010000
    DEBUG:bitcoin:Starting "settxfee" JSON-RPC request
    DEBUG:bitcoin:Got 36 byte response from server in 4 ms
    DEBUG:cointipbot:CtbCoin::__init__(): connecting to Primecoin...
    DEBUG:bitcoin:Read 5 parameters from /opt/altcointip/coins/primecoin/primecoin.conf
    DEBUG:bitcoin:Making HTTP connection to 127.0.0.1:18772
    INFO:cointipbot:CtbCoin::__init__():: connected to Primecoin
    INFO:cointipbot:Setting tx fee of 0.010000
    DEBUG:bitcoin:Starting "settxfee" JSON-RPC request
    DEBUG:bitcoin:Got 36 byte response from server in 1 ms
    DEBUG:cointipbot:CtbCoin::__init__(): connecting to Megacoin...
    DEBUG:bitcoin:Read 5 parameters from /opt/altcointip/coins/megacoin/megacoin.conf
    DEBUG:bitcoin:Making HTTP connection to 127.0.0.1:17950
    INFO:cointipbot:CtbCoin::__init__():: connected to Megacoin
    INFO:cointipbot:Setting tx fee of 0.010000
    DEBUG:bitcoin:Starting "settxfee" JSON-RPC request
    DEBUG:bitcoin:Got 36 byte response from server in 1 ms
    DEBUG:cointipbot:CtbCoin::__init__(): connecting to Litecoin...
    DEBUG:bitcoin:Read 5 parameters from /opt/altcointip/coins/litecoin/litecoin.conf
    DEBUG:bitcoin:Making HTTP connection to 127.0.0.1:19332
    INFO:cointipbot:CtbCoin::__init__():: connected to Litecoin
    INFO:cointipbot:Setting tx fee of 0.020000
    DEBUG:bitcoin:Starting "settxfee" JSON-RPC request
    DEBUG:bitcoin:Got 36 byte response from server in 2 ms
    DEBUG:cointipbot:CtbCoin::__init__(): connecting to Namecoin...
    DEBUG:bitcoin:Read 5 parameters from /opt/altcointip/coins/namecoin/namecoin.conf
    DEBUG:bitcoin:Making HTTP connection to 127.0.0.1:18336
    INFO:cointipbot:CtbCoin::__init__():: connected to Namecoin
    INFO:cointipbot:Setting tx fee of 0.010000
    DEBUG:bitcoin:Starting "settxfee" JSON-RPC request
    DEBUG:bitcoin:Got 36 byte response from server in 1 ms
    DEBUG:cointipbot:CtbCoin::__init__(): connecting to Bitcoin...
    DEBUG:bitcoin:Read 5 parameters from /opt/altcointip/coins/bitcoin/bitcoin.conf
    DEBUG:bitcoin:Making HTTP connection to 127.0.0.1:18332
    INFO:cointipbot:CtbCoin::__init__():: connected to Bitcoin
    INFO:cointipbot:Setting tx fee of 0.000100
    DEBUG:bitcoin:Starting "settxfee" JSON-RPC request
    DEBUG:bitcoin:Got 36 byte response from server in 1 ms
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange crypto-trade.com
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange www.bitstamp.net
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange bter.com
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange blockchain.info
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange campbx.com
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange vircurex.com
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange pubapi.cryptsy.com
    DEBUG:cointipbot:CtbExchange::__init__(): initialized exchange btc-e.com
    DEBUG:cointipbot:CointipBot::connect_reddit(): connecting to Reddit...
    INFO:cointipbot:CointipBot::connect_reddit(): logged in to Reddit as ALTcointip
    ...
    
ALTcointip bot is configured by default to append INFO-level log messages to `logs/info.log`, and WARNING-level log messages to `logs/warning.log`, while DEBUG-level log messages are output to the console.

### Cron: Backups

Backups are very important! The last thing you want is losing user wallets or record of transactions in the databse. 

There are three simple backup scripts included that support backing up database, wallets, and configuration files to local directory and (optionally) to a remote host with `rsync`. Make sure to schedule regular backups with cron and test whether they are actually performed. Example cron configuration:

    0 8,20 * * * cd /opt/altcointip/altcointip/src && python _backup_db.py ~/backups
    0 9,21 * * * cd /opt/altcointip/altcointip/src && python _backup_wallets.py ~/backups
    0 10 * * * cd /opt/altcointip/altcointip/src && python _backup_config.py ~/backups

### Cron: Statistics

ALTcointip bot can be configured to generate tipping statistics pages (overall and per-user) and publish them using subreddit's wiki. After you configure and enable statistics in configuration, add the following cron job to update the main statistics page periodically:

    0 */3 * * * cd /opt/altcointip/altcointip/src && python _update_stats.py
    
### What If I Want To Enable More Cryptocoins Later?

If you want to add a new cryptocoin after you already have a few registered users, you need to retroactively create the new cryptocoin address for users who have already registered. See [src/_add_coin.py](src/_add_coin.py) for details.
