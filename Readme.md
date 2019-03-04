# Create Folder for BTCPay 
mkdir BTCPayServer
cd BTCPayServer

# Download BTCPay 
git clone https://github.com/btcpayserver/btcpayserver-docker
cd btcpayserver-docker

# Set Variables for BTCPay (In this instance, I have BTC, LTC, Dash and Dogecoin), Lightning is via LND
	
export BTCPAY_HOST="btcpay.EXAMPLE.com"
export NBITCOIN_NETWORK="mainnet"
export BTCPAYGEN_CRYPTO1="btc"
export BTCPAYGEN_CRYPTO2="ltc"
export BTCPAYGEN_CRYPTO3="dash"
export BTCPAYGEN_CRYPTO4="doge"
export BTCPAYGEN_REVERSEPROXY="nginx"
export BTCPAYGEN_LIGHTNING="lnd"

# Are we pruning? (Optional), Skip for now)
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-save-storage-xs"

# Install BTCPay 
. ./btcpay-setup.sh -i

# Shut down for moving folders
btcpay-down.sh

# Open up Ports
# BTC 
sudo ufw allow 43782
sudo ufw allow 39388
sudo ufw allow 9735

# LTC
sudo ufw allow 43782
sudo ufw allow 39388
sudo ufw allow 8080
sudo ufw allow 9736

# DASH
sudo ufw allow 9998
sudo ufw allow 9999

# DOGECOIN
sudo ufw allow 22555
sudo ufw allow 22556

# Move Folders to Mnt (Not yet complete)

# Dash 	
mv generated_dash_datadir /mnt/dash/generated_dash_datadir
ln -s /mnt/dash/generated_dash_datadir generated_dash_datadir

# Doge 
mv generated_dogecoin_datadir /mnt/dogecoin/generated_dogecoin_datadir
ln -s /mnt/dogecoin/generated_dogecoin_datadir generated_dogecoin_datadir





# Bring BTCPay Back Up
~/BTCPayServer/btcpayserver-docker/btcpay-up.sh
