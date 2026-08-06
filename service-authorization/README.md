# Service authorization

A signed authorization token for the iFlySEO plugin. The plugin runs only while
this token is valid and signed by a private key held offline. It is renewed
automatically every day by a scheduled workflow, and can be revoked at any time.
The token is public by design — its security is the Ed25519 signature, not
secrecy. This is a fail-closed kill switch: stop renewing and every plugin stops
on its own when the last token expires.
