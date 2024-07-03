Oberon Engineering Pty Ltd
==========================

Mail Server DNS configuration documentation.

# Setup MX

The following DNS MX entries are required within the Zone file;

```
obeeng.com.au. 	14400 	MX 	 10  mail.protonmail.ch
obeeng.com.au. 	14400 	MX 	 20 mailsec.protonmail.ch
```

with the following CNAME entries for the DKIM selectors;

```
protonmail._domainkey.obeeng.com.au. 	14400 	CNAME 	protonmail.domainkey.dblyz2kgbkm6yrkpbhgatxactw46y2x6kdui5hayb6cga7fbzprpq.domains.proton.ch
protonmail2._domainkey.obeeng.com.au. 	14400 	CNAME 	protonmail2.domainkey.dblyz2kgbkm6yrkpbhgatxactw46y2x6kdui5hayb6cga7fbzprpq.domains.proton.ch
protonmail3._domainkey.obeeng.com.au. 	14400 	CNAME 	protonmail3.domainkey.dblyz2kgbkm6yrkpbhgatxactw46y2x6kdui5hayb6cga7fbzprpq.domains.proton.ch
```

and finally the following TXT record;
```
obeeng.com.au. 	14400 	TXT 	protonmail-verification=a99598779c1961323476905fdbd5ee96d3a0b85a
```

That's it!

--
Author: Edward O'Callaghan.
