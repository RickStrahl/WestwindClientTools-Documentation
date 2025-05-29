Allows you to add a client certificate for the request to allow for TLS connections with non-public certificates.


The certificate should be a `.cer` or `.p12` file - not a `.pem` file.

```foxpro
loFtp = CREATEOBJECT("wwFtpClient")
loFtp.lUseTls = .t.
loFtp.cServer = "ftp.myserver.com:23"

loFtp.AddCertificate("