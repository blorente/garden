---
publish: true
created: 2026-01-16T15:52:02.994+00:00
modified: 2026-07-16T10:32:49.482+01:00
---

Links: [[Cryptography]], [[Authentication]], [[Certs]], [[TLS]]
Date: 2024-02-06
Visibility (remove one):

- [[Public]]

---

# Public Key Certificates (.pem certs)

It's a file that says "this public key is valid"
Includes the key itself, as well as info about it:

- What the key is
- Who owns it (the **subject**)
- The digital signature of the CA that has verified the cert (the **issuer**).

Someone can use the public key cert to verify that this public key belongs to this subject, according to this issuer.

## TLS certs

TLS certs for servers are different from the ones for clients.

- Server certs are valid for a range of **hostnames**
- Client certs usually identify an individual, but they can also identify a hostname.

## Self-Signed certs

A self-signed cert is one where the **subject** matches the **issuer**.

- For instance, Borja signs a cert saying that Borja is Borja
- This is also used for the certs from root CAs, because you gotta trust someone.

## X.509

This is the format of most of the TLS certs.
They bind a **subject**'s **identity** to their **public key** using a **signature**, usually from a CA or self-signed.

### Sources

- https://en.wikipedia.org/wiki/Public\_key\_certificate
