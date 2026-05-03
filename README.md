# smtpd v0.1.0

* Repo: https://github.com/pepa65/smtpd
* License: GPLv3
* After: https://github.com/siebenmann/smtpd

Smtpd is a Go package for handling the server side of the SMTP protocol.
It does not handle high level details like what addresses should be
accepted or what should happen with email once it has been fully received;
those decisions are instead delegated to whatever is driving smtpd.
Smtpd's purpose is simply to handle the grunt work of a reasonably
RFC compliant SMTP server, taking care of things like proper command
sequencing, TLS, and basic correctness of some things.

The standard Go library `net/smtp` package handles only the client side
of SMTP; it makes no attempt to provide the facilities you'd need to
write a server.

The package `github.com/pepa65/sinksmtp` depends on `smtpd`, and just
logs and optionally writes it to disk.

Because of its origin as the core engine of a sinkhole SMTP server,
`smtpd` is pretty casual about a lot of things in the SMTP protocol
and in what information it hands to higher layers; for example, it
basically ignores SMTP parameters on MAIL FROM and RCPT TO. It will
accept far longer command lines than are required by the RFC, it has
shorter timeouts than the RFC requires (although you can change that),
and it is somewhat slapdash in doing basic sanity checking on addresses
(the author declines to write an RFC 5321 address parser just to be
fully correct, and yes it's a pity that this part of net/mail is not
exported and no, we're not faking things up so we can use net/mail's
version). These are all defects but the odds of the author fixing them
are low because his sinkhole SMTP server doesn't currently need them.

Smtpd supports PIPELINING by paying no attention to people who do it
anyways and supports STARTTLS if you provide a certificate and a key.
It advertises 8BITMIME and accepts the relevant MAIL FROM parameter;
regardless of what clients do, messages are received in all 8 bits.
It rejects VRFY and EXPN attempts. It supports AUTH if you configure
one or more authentication methods.

References:
* http://tools.ietf.org/html/rfc5321
* http://golang.org/pkg/net/smtp/

Credits:
* Original author: Chris Siebenmann <github.com/siebenmann>
* SMTP AUTH support: Felix Lange <github.com/fjl>

