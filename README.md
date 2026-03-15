# Tesla Fleet API Shortcuts

Use these Apple Shortcuts to authenticate with the Tesla Fleet API, register your app, and retrieve vehicle status data.

<img width="600" alt="Tesla Shortcuts screenshot" src="https://github.com/user-attachments/assets/9c9db925-8883-4419-9a1c-20380e1879f0" />

## What this repo includes

- A static callback page at `v1/callback/index.html`
- Exported Apple Shortcuts in [`Shortcuts`](./Shortcuts)
- Setup steps for hosting the callback URL and installing the shortcuts

## Prerequisites

Before you begin, you will need:

- A Tesla developer account at `https://developer.tesla.com`
- A GitHub account and repository
- A public HTTPS URL for your Tesla app callback
- Apple Shortcuts on iPhone, iPad, or Mac

## High-level flow

The setup process works like this:

1. Create a Tesla developer app.
2. Generate and publish your Tesla app public key.
3. Host the callback page from this repo.
4. Install the shortcuts.
5. Run the setup shortcuts in order.
6. Use the status shortcuts after setup is complete.

## 1. Create a Tesla developer app

Go to `https://developer.tesla.com` and create a new application.

Use these settings:

- OAuth Grant Type: `Authorization Code and Machine-to-Machine`
- Allowed Origin URL: `https://app.host.com`
- Allowed Callback URI: `https://app.host.com/v1/callback`

Enable these API scopes:

- Vehicle Information
- Vehicle Location
- Vehicle Commands
- Vehicle Charging Management

Tesla Fleet API docs:

- `https://developer.tesla.com/docs/fleet-api/getting-started/what-is-fleet-api`

## 2. Generate a Tesla app public key

Create a folder for your keys:

```bash
mkdir -p ~/Documents/tesla-key
cd ~/Documents/tesla-key
```

Generate a private key:

```bash
openssl ecparam -name prime256v1 -genkey -noout -out tesla_private.pem
```

Generate the public key from the private key:

```bash
openssl ec -in tesla_private.pem -pubout -out com.tesla.3p.public-key.pem
```

Optional: inspect the generated public key:

```bash
cat com.tesla.3p.public-key.pem
```

Important:

- Keep `tesla_private.pem` private.
- Only publish `com.tesla.3p.public-key.pem`.

## 3. Publish the callback page

This repo is intended to be hosted with GitHub Pages.

### Add the Tesla app public key

Create this file in your repo:

- `.well-known/appspecific/com.tesla.3p.public-key.pem`

Paste the contents of your generated public key into that file.

### Enable GitHub Pages

In your GitHub repository:

1. Open `Settings -> Pages`
2. Choose the branch to publish
3. Save
4. Set your custom domain, if you are using one
5. Enable HTTPS

This repo also expects:

- `.nojekyll` at the repo root
- `CNAME` at the repo root if you are using a custom domain

The callback page should be available at:

- `https://app.host.com/v1/callback`

If you want to use the callback page included in this repo, see:

- [`v1/callback/index.html`](./v1/callback/index.html)

## 4. Optional: use a custom domain

If you want a custom domain, configure DNS with your registrar.

Example DNS record:

- Type: `CNAME`
- Name: `tesla`
- Data: `<username>.github.io.`
- TTL: `default`

Then your GitHub Pages site can resolve to something like:

- `https://tesla.example.com`

Note:

- A custom domain is optional.
- You may be able to use the default GitHub Pages domain instead, as long as the callback URL configured in Tesla matches your published callback page.

## 5. Install the shortcuts

You can install each shortcut either from its iCloud link or from the exported `.shortcut` file in this repo.

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

## 6. Run the setup shortcuts in order

Run the shortcuts in this order.

### Tesla Setup

This shortcut collects your configuration values.

Be ready to enter:

- Tesla app client values from your Tesla developer account
- Your callback domain and callback URL
- Your preferred vehicle name from the Tesla app
- Any optional behavior flags, such as wake-related settings

### Tesla Setup Authorization

This opens the Tesla authorization page so you can approve access for your app.

### Tesla Setup Get Tokens

After Tesla redirects to your callback page:

1. Open the page in Safari
2. Open the share sheet
3. Send the page to `Tesla Setup Get Tokens`

This shortcut extracts the authorization code and exchanges it for tokens.

### Tesla Setup Register Partner

Run this after tokens are available.

This registers your app in your Tesla region using the Fleet API.

## 7. Use the shortcuts

After setup is complete:

- `Tesla Get Vehicle Status` retrieves vehicle status data
- `Tesla Refresh Tokens` refreshes the Tesla access token
- `Tesla Hello World` is a simple example that calls the vehicle status shortcut and checks whether the car is charging

## Notes about configuration

These shortcuts are designed to use a config-based setup rather than hard-coded values.

That means:

- The exported shortcuts are easier to publish safely
- Secrets should live in your local config and token flow, not in the shortcut definitions themselves
- If you publish this repo, do not commit live secrets or populated private config data

## Troubleshooting

If setup fails, check these items first:

- The callback URL in Tesla exactly matches your hosted `v1/callback` URL
- GitHub Pages is published and reachable over HTTPS
- Your public key file exists at `.well-known/appspecific/com.tesla.3p.public-key.pem`
- The Tesla developer app has the correct OAuth grant type and scopes
- You completed the authorization flow and shared the callback page with `Tesla Setup Get Tokens`

If status requests fail for sleeping vehicles:

- Wake the vehicle before requesting status
- Wait a few seconds after wake before retrying
- Add retry logic if needed in the status shortcut
