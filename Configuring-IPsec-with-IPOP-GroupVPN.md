## Overview

IPOP supports transport-layer security (using SSL's DTLS); for most use cases, this is a simple and sufficient approach to enforce privacy and authentication in communication among endpoints.

There are use cases where IPsec over IPOP may be called for - instead of, or in addition to, DTLS-layer security. Because IPOP works at the IP layer, IPsec works unmodified on top of it. However, configuration of IPsec is typically more complex than DTLS because it often requires setup of different frameworks (e.g. racoon, or strongswan) which come with their own idiosyncrasies.

## IPsec over GroupVPN with Racoon using X.509 certificates on Debian/Ubuntu Linux

IPsec can be tunneled over IPOP to provide end-to-end security within a virtual network. The general approach for configuration is to use Public Key Infrastructure (PKI), where a Certificate Authority (CA) is tasked with signing host certificates for all nodes connected to an IPOP GroupVPN. The following steps demonstrate how to configure IPsec for IPOP/GroupVPN using racoon on Debian/Ubuntu

## Install and configure racoon

1.  First, we need to install racoon

    ```
    apt-get install racoon
    mkdir /etc/racoon/certs
    cd /etc/racoon
    mv racoon.conf racoon.conf-original
    ```

2.  Create a racoon configuration file /etc/racoon/racoon.conf. 

This configuration varies depending on policies you would like for your security framework; the following is one example that uses X.509 certificates:

    ```
    path pre_shared_key "/etc/racoon/psk.txt";
    path certificate "/etc/racoon/certs";
    
    remote anonymous {
            proposal {
                    encryption_algorithm 3des;
                    hash_algorithm sha1;
                    authentication_method rsasig;
            dh_group modp1024;
            }
            certificate_type x509 "host-cert.pem" "host-key.pem";
            my_identifier asn1dn ;
            verify_identifier off;
            exchange_mode main;
    }
    sainfo anonymous {
         authentication_algorithm hmac_sha1;
         compression_algorithm deflate;
            pfs_group modp1024;
            encryption_algorithm 3des;
    }
    ```

3. Configuring IPsec policies

There is one configuration file (ipsec-tools.conf) which needs to be setup with policies indicating that traffic within the IPOP/GroupVPN address space is subject to IPsec. Therefore, this file needs to be configured dynamically based on the IP address range of the GroupVPN. For instance, assuming a private address space of 192.168.0.0/16, this file should be configured as follows:

    ```
    spdadd 192.168.0.0/16 192.168.0.0/16 any -P out ipsec
            esp/transport//require;
    spdadd 192.168.0.0/16 192.168.0.0/16 any -P in ipsec
            esp/transport//require;
    ```

4. Putting certificates, keys in the right place

There are three files that need to be setup in /etc/racoon/certs, per node. the CA certificate file (cacert.pem), the host certificate file (host-cert.pem), and the host's private key (host-key.pem). Note, the host certificate needs to have the subjectAltName extension set. You can use tools such as [https://www.openssl.org/docs/HOWTO/certificates.txt OpenSSL to create and sign certificates] - this is not covered in this documentation. For GroupVPN, you may decide to configure Racoon such that it does not verify subjectAltName against a host name or IP address; in essence, any host with a certificate signed by the trusted CA is allowed into the VPN.

5. Starting things up

The IPsec Racoon service needs to be started up after IPOP is started, as follows:

    ```
    /etc/ipsec-tools.conf
    /etc/init.d/racoon restart
    ```
