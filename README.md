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
- Tesla Setup
  [Install via iCloud](https://www.icloud.com/shortcuts/c5f5b142e20d4c96909060c309f457f6)
  [Download exported shortcut](./Shortcuts/Tesla%20Setup.shortcut)
- Tesla Setup Authorization
  [Install via iCloud](https://www.icloud.com/shortcuts/78150c250fae42d5b341926ac86253b9)
  [Download exported shortcut](./Shortcuts/Tesla%20Setup%20Authorization.shortcut)
- Tesla Setup Get Tokens
  [Install via iCloud](https://www.icloud.com/shortcuts/8e987df1543046b28c6d8b060e3ddc53)
  [Download exported shortcut](./Shortcuts/Tesla%20Setup%20Get%20Tokens.shortcut)
- Tesla Setup Register Partner
  [Install via iCloud](https://www.icloud.com/shortcuts/c0d3d6e5245f4005a2f3e434bcfeb854)
  [Download exported shortcut](./Shortcuts/Tesla%20Setup%20Register%20Partner.shortcut)
- Tesla Get Vehicle Status
  [Install via iCloud](https://www.icloud.com/shortcuts/8094de32e1f24e4b830aaf2e647fe543)
  [Download exported shortcut](./Shortcuts/Tesla%20Get%20Vehicle%20Status.shortcut)
- Tesla Refresh Tokens
  [Install via iCloud](https://www.icloud.com/shortcuts/44e3c4e87b404264b8e79d09a976f09a)
  [Download exported shortcut](./Shortcuts/Tesla%20Refresh%20Tokens.shortcut)
- Tesla Hello World
  [Install via iCloud](https://www.icloud.com/shortcuts/d32e1b9f4ad84e21b52e4b01ff2fa1c6)
  [Download exported shortcut](./Shortcuts/Tesla%20Hello%20World.shortcut)

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
