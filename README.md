# zmssl

```
zmssl v0.2 by annahri
CLI tool for automating Let'sEncrpt SSL certificate issuance and deployment for Zimbra. (Initially written for Zimbra >= 9).
This script requires "certbot" as the ACME client.

USAGE: zmssl [options] <action> -d ...

ACTIONS:
  run           Gets the cert + chain and deploys it.
  cron          Same as run but checks for expiry first. 
  deploy        Only deploy existing cert or arbitrary one (via --cert).
  get-cert      Only gets the certificate.
  check-expiry  Checks the cert expiry remaining days.
  build-chain   Build the certificate chain.

OPTIONS:
  -d --domain <domain>  Domain names. Can be set multiple times for SAN. Required for: run, cron.
  -c --cert <file>      Specify custom certificate file instead of LE's live cert.
  -C --chain <file>     Specify custom chain file instead of LE's live chain bundle.
  -p --priv <file>      Specify custom private key file instead of LE's live privkey. 
  -n --name <name>      Set the name of the certificate. Default: zimbra-ssl.
  -e --email <address>  Specify email address for ACME. Optional.
  -w --days <int>       Set the days within renewal. Cannot be higher than 30. Dafault: 14 (2 weeks)
  
  --force-getcert       Force to get the certificate even if it's not within renewal days.
  --force-getchain      Force to "re-create" the chain bundle.
  --noconfirm           Auto approve all prompts.

  -h --help             Display this help information.

LIMITATIONS:
  Currently, the only supported domain control validation method is HTTP validation. 

EXAMPLES:
  Interactively get a certificate named "production" for mail.example.com and mail.example.id, 
  deploys it and restarts Zimbra services. Also send expiry notifications to admin@example.com:

    ./zmssl run -n production -d mail.example.com -d mail.example.id -e admin@example.com

  Certbot will store the certificates in /etc/letsencrypt/live/production

  Deploy a certificate using custom name:
    ./zmssl deploy -n custom-name

  Sample cron configuration to do renewal every 30 days, weekly checking:
  50 23 * * 6 /path/to/zmssl cron -n <pre-existing-le-cert-dir> -d <domains ...> -e <mail> -w 30

EXTRA:
  For testing purposes, set ZMSSL_STAGING environment variable to true.
```

## TODO

- Mail notification upon failures/successes
- Fix bugs!!
