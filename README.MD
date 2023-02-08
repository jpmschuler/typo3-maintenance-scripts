# TYPO3 maintenance scripts

Doing DevOps for TYPO3 requires some tasks which might be tedious or out-of-scope for normal integrators, but still need
to be done. These scripts might ease that tasks.

In general these scripts are tested on current macOS, sometimes also on current Ubuntu. If you stumble over anything
feel free to fire up an issue in order to make them more compatible or clear up prerequisites (e.g. an SSL checker needs
openssl installed).

## scripts

### &gt; typo3-domains-ssl-check-short

Run this in your TYPO3 instance dir. It scans `config/sites/` for lines starting with `base:` and extracts the domain
names. Then fires up curl and openssl to check if the cert is valid (e.g. installed correctly with valid chain) and how
long until the certs expire. Warns if <30 days, errors if <7 days.
Prerequisites:

- `typo3-get-domains-from-siteconfig` shows valid output

## helper scripts

### &gt; typo3-get-domains-from-siteconfig

Run this in your TYPO3 instance dir. It scans `config/sites/` for lines starting with `base:` and extracts the domain
names. As basevariants are prefixed with a dash they won't show up, so this only covers PRODUCTION. Just interprets
plain text. Thus, YAML includes and ENV vars won't be supported.

### &gt; ssl-check-short &lt;domain&gt;

Fires up curl and openssl to check if the cert is valid (e.g. installed correctly with valid chain) and how
long until the certs expire. Warns if <30 days, errors if <7 days. Concise, one-line output.

Will show a problem if
- cert domain not matching
- cert expired
- no connection possible (e.g. DNS or server issues)

Won't show a problem if e.g. httpauth protected (although in general a curl error, this is suppressed)

### &gt; ssl-check &lt;domain&gt;

Fires up curl and openssl to check if the cert is valid (e.g. installed correctly with valid chain). Additionally shows
SAN/SNI domains, expiry, issuer, certification path and fingerprints.