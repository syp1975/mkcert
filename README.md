# mkcert

A pure _bash script_ that generates self-signed certificates.

Certificates are signed with a custom CA root certificate.
Install this CA root certificate in your computer as a Trusted Root Certification Authority once, then all generated self-signed certifcates will be trusted by your browser. Usefull for web development or home labs.

> If a CA root certificate is not found, one will be generated automatically.<br>CA root certificate is not renewed automatically.

## Usage

`mkcert <domain> [<domain|ip> ...]`<br>
Generate a self-signed certificate for the given domain(s) or IP address(es).

## Examples

```sh
#creates example.com.key and example.com.crt in current folder
mkcert example.com *.example.com

#creates
#/etc/ssl/my_home/CA.key (password protected, password is stored in /etc/ssl/my_home/.password),
#/etc/ssl/my_home/CA.crt (copy this file to your computer and install it as trusted root CA)
#/etc/ssl/my_home/my_router.home.key (use this cert on your web server)
#/etc/ssl/my_home/my_router.home.crt
DAYS=1825 ORGANIZATION=/etc/ssl/my_home CA_PASSWORD=file:.password mkcert my_router.home 192.168.1.1
```

## Variables

Pass options to the script using environment variables.

### For self-signed certificate generation

- **`DAYS`**: number of days the certificate is valid for (730).
- **`ORGANIZATION`**: path to the folder where certificates will be stored.<br>
  Defaults to the current folder.<br>
  Folder will be created if it does not exist.<br>
  Folder name will be used as the organization name.
- **`CA`**: name of the root CA certificate files (CA).
- **`CA_PASSWORD`**: password to access the private key of the root CA certificate.<br>
  If not set, a password will be prompted when needed.<br>
  Examples:

  - `pass:123456`
  - `env:MyCAPassEnvVar`
  - `file:/path/to/protected/file/with/password`

- **`RENEW`**: days before expiration that an existing certificate will be renewed.<br>
  If no set, allways replaces existing certificates.
- **`ENV`**: path to a file containing environment variables.
- **`BITS`**: number of bits in the generated private keys (2048).

### For CA root certificate generation

- **`COUNTRY`**: name of the country (US).
- **`STATE`**: name of the state (CA).
- **`LOCALITY`**: name of the locality (Home).
- **`CA_DAYS`**: number of days the root CA certificate is valid for (3650).
- **`CA_ENCRYPTION`**: encryption algorithm for the root CA certificate (des3).
- **`CA_EXPIRATION`**: days before CA certificate end date that a warning will be issued (30).

## Files

Variables can be placed in:

- `/etc/mkcert`
- `.mkcert` in `ORGANIZATION` folder.
- In defined `ENV` file.
- Passed in the command line.

> Do not put a plain password in the command line.<br>`ps` command will reveal it.

## Dependencies

- The script uses _openssl cli tool_ to generate and sign certificates.

## License

_mkcert_ is released under the MIT License.
