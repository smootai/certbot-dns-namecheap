# General

This plugin automates the process of completing a `dns-01` challenge by creating, and subsequently removing, TXT records using the (XML-RPC-based) namecheap.com API.

---

## Presequence

### Getting API access

Namecheap has certain requirements for activation to prevent system abuse. In order to have API enabled for your account, you should meet one of the following requirements:

- have at least 20 domains under your account;
- have at least $50 on your account balance;
- have at least $50 spent within the last 2 years.

## Credentials

Use of this plugin requires a configuration file containing Namecheap API credentials, obtained from your Namecheap account's [API Managenment page](https://ap.www.namecheap.com/settings/tools/apiaccess/).

```ini
# Namecheap API credentials used by Certbot
certbot_dns_namecheap:dns_namecheap_username=my-username
certbot_dns_namecheap:dns_namecheap_api_key=my-api-key
```

The path to this file can be provided by using the `--certbot_dns_namecheap:dns-namecheap-credentials` command-line argument.

## Usage

### Docker

- **Recommended usage**. Create the credentials file and 2 folders for the certificates and logs and run:

```sh
docker run -it --rm \
  -v $(pwd)/certs:/etc/letsencrypt \
  -v $(pwd)/logs:/var/log/letsencrypt \
  -v $(pwd)/namecheap.ini:/namecheap.ini \
  schubc/certbot-dns-namecheap certonly \
  -a dns-namecheap \
  --dns-namecheap-credentials=/namecheap.ini \
  --agree-tos \
  --no-eff-email \
  -email "your@mail.com" \
  -d example.com \
  --dry-run
```

- After a successful run, remove the last parameter `--test-cert` which enabled [staging server](https://letsencrypt.org/docs/staging-environment/) and run again.

## Python

- If you know what you're doing install the plugin into the same python environment like `certbot`. In any other case follow the `Docker` approach above:

```sh
git clone https://github.com/smootai/certbot-dns-namecheap.git
pip install certbot-dns-namecheap/
```

- Check that `certbot` discovers the plugin:

```sh
certbot plugins
```

- Now run the command:

```sh
certbot certonly \
  -a dns-namecheap \
  --dns-namecheap-credentials=/namecheap.ini \
  --agree-tos \
  --no-eff-email \
  -email "your@mail.com" \
  -d example.com \
  --dry-run
```
