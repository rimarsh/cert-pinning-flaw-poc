# cert pinning flaw POC

Simple POC script for testing CVE-2016-2402 and similar flaws. Read [my blog post](https://koz.io/pinning-cve-2016-2402) for details.

This utility will set up a HTTPS server that servers a malicious certificate chain to the client for a specific domain.

If traffic from an app with a vulnerable certificate pinning implementation is redirected to this server, 
the pinning control will be bypassed and you should be able to see a GET or a POST request in the server console.

By default, this uses a hardcoded CA certificate and key (CA_CERT.pem and CA_KEY.pem files).

You can change these, use the following command to generate a new pair.

`openssl req -x509 -days 1825 -nodes -newkey rsa:2048 -outform pem -keyout CA_KEY.key -out CA_CERT.pem`

You will want to insert CA_CERT.pem to the platform being tested.

John Kozyrakis

### Usage

1. Install python3 `brew install python3` and `pip3 install pyopenssl`
1. Run the script to configure itself:  
    `sudo python3 cert-pinning-flaw-poc.py -d <host to attack> -m attack -v`
1. Edit `/etc/hosts` to include:  
    `127.0.0.1 <host to attack>`
1. Run tests and see output here if the attack is successful.

