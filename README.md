> By contributing to this project, you agree to abide by our [Code of Conduct](https://github.com/freedomofpress/.github/blob/main/CODE_OF_CONDUCT.md).

# HTTPS-Everywhere Rulesets for SecureDrop

`securedrop-https-everywhere-ruleset` is used to create a signed HTTPS Everywhere ruleset that maps full-length .onion addresses to user-friendly [onion names](https://securedrop.org/faq/getting-onion-name-your-securedrop/) for some news organizations listed in the [SecureDrop directory](https://securedrop.org/directory/). Any time a new onion name is approved, we add its mapping to our HTTPS Everywhere ruleset and deploy it to https://securedrop.org/https-everywhere/ . Tor Browser automatically includes our ruleset in the default HTTPS Everywhere extension and checks for updates on startup. (Tor Browser will soon switch to checking https://securedrop.org/https-everywhere-2021/ which uses our new release signing key).

## Development

Setup:

```
virtualenv --python=python3 .venv
source .venv/bin/activate
pip install --require-hashes --no-deps -r requirements.txt
```

You can create a test key for signing using:

```
make test-key
```

which will create `test-key.jwk` in your current working directory.

## Updating Rulesets

### Adding a new organization

1. Ensure they are in the official SecureDrop directory. If they are not, go through the IVF process with the organization.

2. Add their domain name and the requested URL to the `onboarded.txt` via PR into this repository. We match the domain based on the landing page of the organization, comparing the `netloc` in a URL with structure `scheme://netloc/path;parameters?query#fragment`.

3. Next, generate and sign the update ruleset using the following command (requires signing key, please ping a key holder for assistance):

```
./scripts/generate-and-sign
```

4. Commit all files generated by the script above and open a Pull Request to this repository. Once the PR is merged, the rulesets will automatically be deployed to production.

## Verifying changes

Inspect the diff. If it looks good, commit the resulting `index.html` and all files to be served. To test locally, run

    make serve

And configure your browser to use `http://localhost:4080/https-everywhere/`.

## Deployment

Upon merge the container will be published to `quay.io/freedomofpress` and the new tag will be deployed automatically.
