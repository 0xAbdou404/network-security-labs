# Lab 01: HTTP vs HTTPS vs DNS in Wireshark

**Status**: In progress

## Goal
See what an observer on the network can read in DNS, HTTP and HTTPS traffic, and what encryption hides.

## Environment
Ubuntu VM(VMware, NAT), Docker Engine, Wireshark

## Phases
### Phase 1: DNS
- Start Wireshark on the right interface 
- Run a DNS lookup with `dig`
- Apply the display filter `dns`
- Find and note 5 fields: queried name, record type , response, port, transaction ID or TTL
- Run the same lookup a second time and note what changes
### Phase 2: HTTP
- Create the `/site` folder with login page (HTML form with POST
- Write `docker-compose.yml` to serve it with nginx on a port you choose
- Start it and open it in the VM browser
- Decide which interface Wireshark must capture, and test your idea
- Log in with fake credentials, then use Follow TCP Stream
- Find the username and password in plain text, and note the HTTP status code of the response

### Phase 3: HTTPS
- Generate a self-signed certificate with openssl
- Add the key to `.gitignore` and write the generating command in your README
- Add an HTTPS port to the nginx configuration and compose file
- Open the site, and write down why the browser warns you
- Capture again and find: The TCP handshake, ClientHello, the TLS version, the SNI, and the encrypted data
- Check that the credentials are unreadable

## Defense
### HSTS
HSTS is HTTP Strict Transport Security, HTTPS is very secure, but it has one flaw: It's not on by default. An attacker in the middle could hijack a user's connection before you can tell them to use HTTPS. HSTS solves this issue, with HSTS enabled, the server sends the same 301 moved permanently response, but also sends a header saying, "Hey, I don't support HTTP. Don't even try making more HTTP request", the browser get's the message, and will redirect itself to HTTPS before sending anything. This makes sure your site is fully HTTPS, by default, all the time.
### DoH and DoT
- **DoT** mean DNS over TLS is a network protocol security measure designed to provide privacy and data integrity in communication between web browser and DNS resolvers.DoT adds an additional layer of TLS encryption on top of the UDP, it use the port 853
- **DoT** DNS over HTTPS is an alternative to DNS over TLS, that ensures DNS queries and responses are encrypted, and unlike DoT, it sends them via the HTTP or HTTP/2 protocols, is use the port 443, use TCP


## Mistakes and lessons
(From my error log.)
