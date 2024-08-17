Oberon Engineering Pty Ltd
==========================

Misc DNS configuration documentation.

# Setup MyOb email service.

MyOb provide a mailer service that is utterly undocumented for sysadmins. This
results in almost universal misconfigurations where remittances end up in Spam,
Failure to deliever or worse, Spoofed emails from 'The Company' causing fruad.

The following is how to correctly setup MyOb's mailer within your DMARC policy.
The MyOb mailer is actually 'MandrillApp' provided by 'Mailchimp', together
with a delegate DKIM key for MyOb themseleves.

## DKIM Setup

The following three `CNAME` records are needed within your DNS Zone:

```
mandrill._domainkey.obeeng.com.au. 	3600 	CNAME 	mandrill._domainkey.apps.myob.com
mte1._domainkey.obeeng.com.au. 	3600 	CNAME 	dkim1.mandrillapp.com
mte2._domainkey.obeeng.com.au. 	3600 	CNAME 	dkim2.mandrillapp.com
```

The `mandrill` DKIM selector is the delegate key endowed to MyOb by Mailchimp
whereas the `mte1` and `mte2` selectors are Mailchimp's DKIM primary and
secondary keys.

Note that `mte1._domainkey.apps.myob.com` is just a `CNAME` to
`dkim1.mandrillapp.com` and likewise for the `mte2` selector.

## SPF Setup

The SPF TXT record must contain `+include:spf.mandrillapp.com` and *NOT*
`+include:apps.myob.com`, this is because the SPF mechanism uses the
`Return-Path:` header token and *NOT* the `From:` header token to obtain the
FQDN to validate. Also note that `spf.apps.myob.com` is just a `CNAME` to
`spf.mandrillapp.com` anyway.

For Oberon Engineering the `SPF` policy is as follows:

```
obeeng.com.au. 	3600 	TXT 	v=spf1 include:_spf.protonmail.ch include:spf.mandrillapp.com ~all exp=explain._spf.%{d}
explain._spf.obeeng.com.au. 	14400 	TXT 	Emails from obeeng.com.au must only be sent by authorised mail servers. Call Gary.
```

## DMARC Setup

Finally, don't have stupid `DMARC` policies set to `none` or `quarantine`, that
is like a fire alarm that makes no sound.

Here is a reasonable `DMARC` policy DNS entry configuration:

```
_dmarc.obeeng.com.au. 	3600 	TXT 	v=DMARC1;p=reject;sp=reject;adkim=s;aspf=s;pct=100;fo=1;rf=afrf;ri=86400;rua=mailto:c8291de2@in.mailhardener.com;ruf=mailto:c8291de2@in.mailhardener.com
```

## DNSSecurityTXT Setup

Ensure a security contact is in the DNS Zone file with the following entry:

```
_security.obeeng.com.au. 	14400 	TXT 	security_contact=mailto:admin@obeeng.com.au
```

--
Author: Edward O'Callaghan.
