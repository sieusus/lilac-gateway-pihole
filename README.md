# Lilac Gateway Pi-hole (LLGP)

![LLGP Cover](https://molify.net/content/images/size/w2000/2023/10/Personal-custom-Cloudflare-Zero-Trust-DNS-with-Ads-Block-Filter.png)

Cloudflare Gateway allows you to create custom rules to filter HTTP, DNS, and network traffic based on your firewall policies. This is a collection of scripts that can be used to get a similar experience as if you were using Pi-hole, but with Cloudflare Gateway - so no servers to maintain or need to buy a Raspberry Pi!

> **Support for female and low-tech user to fast and minium setup steps**

## For Vietnamese User
Go to [Molify](https://molify.net/tu-tao-dns-voi-cloudflare-zero-trust/)

## About the individual scripts

- `cf_list_delete.js` - Deletes all lists created by LLGP from Cloudflare Gateway. This is useful for subsequent runs.
- `cf_list_create.js` - Takes a blocklist.txt file containing domains and creates lists in Cloudflare Gateway
- `cf_gateway_rule_create.js` - Creates a Cloudflare Gateway rule to block all traffic if it matches the lists created by LLGP.
- `cf_gateway_rule_delete.js` - Deletes the Cloudflare Gateway rule created by LLGP. Useful for subsequent runs.
- `download_lists.js` - Initiates blocklist and whitelist download.

## Features

- **Support for female and low-tech user to fast and minium setup steps**
- Support for basic hosts files
- Full support for domain lists
- Automatically cleans up filter lists: removes duplicates, invalid domains, comments and more
- Works fully unattended
- Whitelist support, allowing you to prevent false positives and breakage by forcing trusted domains to always be unblocked.
- Optional health check: Sends a ping request ensuring continuous monitoring and alerting for the workflow execution.

## Usage 

### Run in GitHub Actions

These scripts can be run using GitHub Actions so your filters will be automatically updated and pushed to Cloudflare Gateway. This is useful if you are using a frequently updated malware blocklist.

Please note that the GitHub Action downloads the recommended blocklists and whitelist by default. You can change this behavior by editing the file.

1. Create a new empty, private repository. Forking or public repositories are discouraged, but supported - although the script never leaks your API keys and GitHub Actions secrets are automatically redacted from the logs, it's better to be safe than sorry.
2. Create the following GitHub Actions secrets in your repository settings:

- `CLOUDFLARE_API_KEY`: Your Cloudflare API key
- `CLOUDFLARE_ACCOUNT_ID`: Your Cloudflare account ID
- `CLOUDFLARE_ACCOUNT_EMAIL`: Your Cloudflare account email
- `CLOUDFLARE_LIST_ITEM_LIMIT`: The maximum number of blocked domains allowed for your Cloudflare Zero Trust plan. Use 300000 for the free plan or if you're unsure.
- `PING_URL`: /Optional/ The HTTP(S) URL to ping (using curl) after the GitHub Action has successfully updated your filters. Useful for monitoring.

3. Create the following GitHub Actions variables in your repository settings if you desire:

- `FAST_MODE`: Enable the scripts to send the requests simultaneously. Beware that there's a rate limit of 1200 requests per five minutes (https://developers.cloudflare.com/fundamentals/api/reference/limits/) so make sure you know what you are doing.
- `ALLOWLIST_URLS`: Uses your own allowlists. One URL per line. Recommended allowlists will be used if this variable is not provided.
- `BLOCKLIST_URLS`: Uses your own blocklists. One URL per line. Recommended blocklists will be used if this variable is not provided.

4. Enable GitHub Actions in your repository settings.

### DNS setup for Cloudflare Gateway

1. Go to your Cloudflare Zero Trust dashboard, and navigate to Gateway -> DNS Locations.
2. Click on the default location or create one if it doesn't exist.
3. Configure your router or device based on the provided DNS addresses.

Alternatively, you can install the Cloudflare WARP client and log in to Zero Trust. This method proxies your traffic over Cloudflare servers, meaning it works similarly to a commercial VPN.

### Dry runs

To see if e.g. your filter lists are valid without actually changing anything in your Cloudflare account, you can set the `DRY_RUN` environment variable to 1, either in `.env` or the regular way. This will only print info such as the lists that would be created or the amount of duplicate domains to the console.

**Warning:** This currently only works for `cf_list_create.js`.

## License

MIT License. See `LICENSE` for more information.

## Credit

This repository modified from source `mrrfv/cloudflare-gateway-pihole-scripts`

