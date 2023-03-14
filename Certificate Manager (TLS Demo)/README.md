# Enable TLS and grab a valid certificate for your domain

This demo will use the Nginx Ingress Controller, so be sure to have it installed before proceeding.

Further this will only work for non-wildcard domains because we'll use the `HTTP Validation Challenge` and not the `DNS Validation` one.

Be careful with the LetsEncrypt Poduction Issuer. There is a rate limit and you can hit it very fast. Keep that in mind - for development environments a LetsEncrypt certificate from the Staging Issuer is enough, even if you'll end up with a warning in your browser.

1. Install [Certificate Manager](https://cert-manager.io/docs/installation/)

2. Grab your public IP address: `kubectl -n ingress-nginx get svc`

3. Add your IP and your E-Mail to the appropriate parts of the .yaml files.

4. Apply all ressources `kubectl apply -f .` or just the staging or production ones, as you like.