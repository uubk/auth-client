# auth-client
[![CircleCI status](https://img.shields.io/circleci/project/github/uubk/auth-client/master.svg?style=shield)](https://circleci.com/gh/uubk/auth-client/tree/master)
![License](https://img.shields.io/github/license/uubk/auth-client.svg?style=popout)

Configure local LDAP and Kerberos clients and optionally deploy keytabs. Tested on Debian 9.

## Configuration
| Name | Default value | Description |
| ---- | ------------- | ----------- |
|`auth_client_kerberos_admin_servers` | `[]` | List of kadmin servers. Since kerberos currently only supports specifying a single admin server, one will be chosen from this list at random. |
|`auth_client_ldap_servers`|`[]`| List of LDAP servers. A single entry will be choosen at random to be used by ansible from the control machine and all entrys will be put into the `ldap.conf` |
|`auth_client_kerberos_kdcs`|`[]`| List of kerberos KDCs. At the moment, they will not be listed in the config, however their names will be listed for anonymous PKINIT |
|`auth_client_ldap_basedn` | None | LDAP base DN |
|`auth_client_ldap_binddn` | None | LDAP admin bind DN |
|`auth_client_ldap_bindpwd` | None | LDAP admin bind password |
|`auth_client_kerberos_admin_user` | None | Kerberos principal with admin rights |
|`auth_client_kerberos_admin_password` | None | Password for the principal listed above |
|`auth_client_kerberos_realm` | None | Kerberos realm (uppercase!) |
|`auth_client_ca` | `/etc/ldap/ca.pem` | Path to LDAP CA (also used for PKINIT) |
|`auth_client_service_accounts` | `[]` | List of service accounts to create, see below. |
|`auth_kerberos_enctypes` | `aes256-cts-hmac-sha384-192 aes128-cts-hmac-sha256-128` | Kerberos enctypes to enable. |

Service accounts (and keytabs) can be created by putting them into `auth_client_service_accounts` as a dict with the following format:
```
auth_client_service_accounts:
  - service: host
    keytab: /etc/krb5.keytab
    ktowner: root
    ktgroup: root
    ktmode: "0600"
```
This would create `host/{{ ansible_fqdn }}@{{ auth_client_kerberos_realm }}` and store it in `/etc/krb5.keytab`.

## License
Apache 2.0
