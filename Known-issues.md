1.  IPOP currently is unable to authenticate to Google accounts' XMPP server if your account is configured with 2-step verification

2.  The Windows port does not currently support DTLS-based encryption and authentication over tincan links - so IPOP gives a virtual network, but it is not private.

3.  Our code requires IPv6 support in the linux kernel in order to function properly.

3.  We've seen segmentation fault crashes in the Windows port. If you run Windows and has any stability problems, please [[report an issue|https://github.com/ipop-project/ipop-tincan/issues]].