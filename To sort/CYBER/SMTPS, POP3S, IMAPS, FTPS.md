Similar to TLS being added to HTTP giving HTTPS, the same is made with SMTPS, POP3S, IMAPS and FTPS.

They all require a proper TLS certificate for control and data transfer.

With the addition of sending data over a TLS connection, the ports used by these new protocols are different from the ones used by the non-secured versions:

- SMTPS: 462 and 587
- POP3S: 995
- IMAPS: 993
- FTPS: 990

