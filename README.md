# How to use iOS Shortcuts to get the status of your Tesla

<img width="600" alt="image" src="https://github.com/user-attachments/assets/9c9db925-8883-4419-9a1c-20380e1879f0" />

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
  - the contents of the file should be the public key
- Configure Github Pages
  - Repo Settings -> Pages
  - enable pages by choosing a source branch and saving
  - enter your custom domain and save
    - for example this repo is configured like `tesla.millimoose.com` (the tesla part will be configured in the DNS record in GoDaddy)
  - enable `enforce http`
- add a file named `.nojekyll` to the root of the repo. this will disable jekyll processing and make it so files and directories are not ignored
- add a file named `CNAME` to the root of the repo. the contents should be your domain name like `tesla.millimoose.com`
- (optional) add a file named `v1/callback/index.html` to the repo. the contents should be [this](https://github.com/dalethomas81/millimoose/blob/main/v1/callback/index.html)

## Get a domain (optional?)
`https://www.godaddy.com`  
note: i'm not totally certain this step is required. it may be possible to use only the Pages feature of Github.  
- configure DNS settings by adding a new DNS record
  - Type: `CNAME`
  - Name: `tesla` (this can be anything and will be used like www.tesla.millimoose.com)
  - Data: `<username>.github.io.`
  - TTL: `default`
- the DNS record can take 15 minutes to start working

## Install Shortcuts
- [Tesla Setup](https://www.icloud.com/shortcuts/b87bf34a7c404f638c1a7e72aeea5076)
- [Tesla Setup Authorization](https://www.icloud.com/shortcuts/83758460d3d94c14acc7a9006772bfb6)
- [Tesla Setup Get Tokens](https://www.icloud.com/shortcuts/b8901757cab7444183c629849254d69c)
- [Tesla Setup Register Partner](https://www.icloud.com/shortcuts/b62ac0bfc27e4e46bbd932a7c93cb7b1)
- [Tesla Get Vehicle Status](https://www.icloud.com/shortcuts/f4ebfe1ac48d4c2c858dae2bc1edc71e)
- [Tesla Refresh Tokens](https://www.icloud.com/shortcuts/c47c0bd967d94a0299a40075985b0f57)
- [Tesla Hello World](https://www.icloud.com/shortcuts/455ccbafc2a441b2be3486fccbf6f620)

## Run Tesla Setup Shortcut
- Enter in values from Developer Account
- Enter in car name from Tesla App

## Run Tesla Setup Authorization
- This will open a web page to authorize the app
- Once this completes, open the share sheet in Safari and share the web page with the Tesla Setup Get Tokens Shortcut

## Run Tesla Setup Register Partner
- This will register the app in your region

## Run Tesla Hello World
- This Shorcut will call the Tesla Get Vehicle Status Shortcut and determine if your car is charging
