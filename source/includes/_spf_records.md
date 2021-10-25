# Custom Email Domains

If you have custom email domains within Learn Amp enabled, use this documentation to configure SPF with SendGrid. Sender Policy Framework (SPF) is an open standard aimed at preventing sender address forgery. We partner with [SendGrid](https://sendgrid.com/why-sendgrid/) to provide us with reliable email delivery. 

## Custom SPF records

If you have an SPF record set for your domain already, you must add a alphanumeric string before the all mechanism of this record in order to authenticate mailings through SendGrid. If you do not have an existing SPF record for your domain, you must create a TXT record with the value provided to you during the domain authentication process. An example of such a record is:

`v=spf1 include:sendgrid.net -all`

In this example, we have an SPF record for the authorization of outbound mail for SendGrid. A -all inclusion versus an ~all inclusion indicates that this SPF record is the only record used to authenticate mail for your domain. Make sure to include any other authorized sender into this SPF record if you need to authenticate mailings from other sources.

Do not create more than one SPF1 record for a given domain. If more than one SPF1 record exists for a domain, you will want to merge any additional SPF records into one SPF record. You also cannot have more than 10 DNS lookups in your single SPF record.

## Working with an existing SPF record

If you already have an SPF record for your domain, you need to add SendGrid SPF inclusion into your existing record.

For example, say your existing record looks like this:

`v=spf1 a mx include:\_spf.google.com include:spf.protection.outlook.com -all`

You would need to add the SendGrid lookup at the end of the string, before the all mechanism, like so:

`v=spf1 a mx include:\_spf.google.com include:spf.protection.outlook.com include:sendgrid.net -all`