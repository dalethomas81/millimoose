# How to

## Create a Tesla developer account
`https://developer.tesla.com`
- Create new application
- OAuth Grant Type should be `Authorization Code and Machine-to-Machine`
- Allowed origin url should be `https://app.host.com`
- Allowed callback uri should be `https://app.host.com/v1/callback`
- API Scopes should be
  - Vehicle Information
  - Vehicle Location
  - Vehicle Commands
  - Vehicle Charging Management

Tesla docs: `https://developer.tesla.com/docs/fleet-api/getting-started/what-is-fleet-api`

## Buy a domain
`https://www.godaddy.com`

## Create a Github account
`https://github.com`
- Create a repo
- Create a public key
  - make folder where ssl key will be stored `mkdir -p ~/Documents/tesla-key`
  - move to created folder `cd ~/Documents/tesla-key`
  - generate private key (DO NOT SHARE THIS!) `openssl ecparam -name prime256v1 -genkey -noout -out tesla_private.pem`
  - generate public key from private key `openssl ec -in tesla_private.pem -pubout -out com.tesla.3p.public-key.pem`
  - optionally read out public key `cat com.tesla.3p.public-key.pem`
- Create pem file in repo `.well-known/appspecific/com.tesla.3p.public-key.pem`
- Configure Github Pages
