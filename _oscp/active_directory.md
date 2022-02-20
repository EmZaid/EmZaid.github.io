---
title: "Active Directory for OSCP"
permalink: /oscp/active_directory/
excerpt: "Lateral and vertical movements in AD"
last_modified_at: 
toc: true
toc_sticky: true
---


# Intro

Active directory is a system to manage users and computers. (think group policy)

An AD network is called a Domain.

# Domain

Every domain has a DNS name. This may be the same as a website

`ruby.com`
or it may be:
`ruby.local`

## NetBios Name
In addition every domain can be identified by a NetBios name:

`ruby`

(think of it as the part before `.local`)


## Security Identifier (SID)
A domain can also be identified by a Security Identifier (SID) as seen below:

```powershell
PS C:\Users\Lizz> Get-ADDomain | select DNSRoot,NetBIOSName,DomainSID

DNSRoot       NetBIOSName DomainSID
-------       ----------- ---------
ruby.local 		  RUBY     S-1-5-21-1372086773-2238746523-2939299801
```

# Forest

## Tree
Think of it like subdomains. Given:
`ruby.local`
you can make things like:
`games.ruby.local`
`sales.ruby.local`

Where games and sales define the subdomains.

This forms a tree where the root domain (`ruby.local`) branches out to other subdomains.

## Forest
In a forest, each domain has its own database and its own Domain Controllers. 

Users in a forest can access other domains in that forest. 

But they are limited by default from accessing other forests.

For example:
a usr in: `ruby.local` would have acess to `games.ruby.local` but would not have access to `games.ruby.tryharder`

To get the forest:
```powershell
PS C:\Users\Lizz> Get-ADForest

ApplicationPartitions : {DC=DomainDnsZones,DC=contoso,DC=local, DC=ForestDnsZones,DC=contoso,DC=local}
CrossForestReferences : {}
DomainNamingMaster    : dc01.contoso.local
Domains               : {contoso.local}
ForestMode            : Windows2016Forest
GlobalCatalogs        : {dc01.contoso.local, dc02.contoso.local}
Name                  : contoso.local
PartitionsContainer   : CN=Partitions,CN=Configuration,DC=contoso,DC=local
RootDomain            : contoso.local
SchemaMaster          : dc01.contoso.local
Sites                 : {Default-First-Site-Name}
SPNSuffixes           : {}
UPNSuffixes           : {}

```

The forest mode tells you that any domain controller is at least that version. Using the above example all domain controllers are at least Windows 2016.

More enumeration commands:
```powershell
PS C:\Users\Administrator\Downloads> (Get-ADForest).ForestMode
Windows2016Forest
PS C:\Users\Administrator\Downloads> (Get-ADDomain).DomainMode
Windows2016Domain
```

See [here](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/564dc969-6db3-49b3-891a-f2f8d0a68a7f) for more info.

[This](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/raise-active-directory-domain-forest-functional-levels) will explain about forest function levels and raising them.

# Trusts

Users can access other domains in the forest because of trusts.

Trust direction is opposite access direction. 


```
(trusting)         trusts        (trusted)
  ruby.local A  -------------------->  games.ruby.local
       outgoing               incoming
       outbound               inbound
                    access
            <--------------------
```

When a trust is directed through your domain it allows your users to access the other domains. 

This can be referred to as an inbound trust or incoming trust.

Outbound/outgoing trusts allow users of another domain to access your domain.

```powershell
PS C:\Users\Administrator> nltest /domain_trusts
List of domain trusts:
    0: ruby ruby.local (NT 5) (Direct Outbound) ( Attr: foresttrans )
    1: ITPOKEMON it.poke.mon (NT 5) (Forest: 2) (Direct Outbound) (Direct Inbound) ( Attr: withinforest )
    2: POKEMON poke.mon (NT 5) (Forest Tree Root) (Primary Domain) (Native)
The command completed successfully
```

In the above, `poke.mon` is the Primary Domain. It has bidirectional trust with `it.poke.mon`. Also users of `ruby.local` can access `poke.mon`.


## Transitive Trusts

Trusts can be transitive.

That is:

given three domains:
`ruby.local`
`giant.tips`
`rough.edge`

and the following trust.

```      (trusting)   trusts   (trusted)  (trusting)   trusts   (trusted)
  ruby.local  ------------------->  giant.tips --------------------> rough.edge
                    access                          access
            <-------------------           <--------------------
```

If the trust between `ruby.local` and `giant.tips` is transitive then users in `giant.tips` and `rough.edge` will have access to `ruby.local`. 

If it is not transitive then only `giant.tips` will have access to `ruby.local`


## Trust Types

There are 4 trust types. See official [documentation](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730798(v=ws.10)#trust-types)

Also see [here](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/concepts-forest-trust)

Parent-Child: Trust between parent domain and child. In same forest.
External: Connects to specific domain not in forest.
Realm: Special trust to connect Windows AD to non Windows domain
Forest: Trust between forests. If this is misconfigured then forests can be taken over.
Shortcut: When two domains within the forest communicate often but are not directly connected.



The trust path is implemented by the Net Logon service using an authenticated remote procedure call (RPC) connection to the trusted domain authority.


## Trust Key

Communication between domain Controllers (from here on out referred to as: DC) requires a key. This can use NTLM, Kerberos, etc. This key is know as the trust key.

When a trust is created a trust account is also created in the domain database as if it were a user (this name ends in $, or at least it did in the past.)

The trust key is stored as the password of the trust account. This is either a NTLM hash or Kerberos key

# Users
User objects stores data about user. One object is the username. This is listed in the SamAccountName attribute.

SID can also be used to identify the user. The users SID is:

Domain SID + User Relative Identifier (RID)

```powershell
PS C:\Users\Lizz> Get-ADUser Lizz


DistinguishedName : CN=Anakin,CN=Users,DC=contoso,DC=local
Enabled           : True
GivenName         : Lizz
Name              : Lizz
ObjectClass       : user
ObjectGUID        : 58ab0512-9c96-4e97-bf53-019e86fd3ed7
SamAccountName    : lizz
SID               : S-1-5-21-1372086773-2238746523-2939299801-1103
Surname           :
UserPrincipalName : lizz@ruby.local
```

RID is the last number in the user SID. In the above example it is 1103.

DistinguishedName is used by LDAP API. If you query LDAP you will probably get things from DistinguishedName

To enumerate users run:

```powershell
PS C:\Users\Lizz> Get-ADUser -Filter * | select SamAccountName

SamAccountName
--------------
Administrator
Guest
krbtgt
POKEMON$
```

The krbtgt account is special and can be used in [[golden tickets]]




```powershell
PS C:\> Get-ADObject -LDAPFilter "objectClass=User" -Properties SamAccountName | select SamAccountName

SamAccountName
--------------
Administrator
Guest
DC01$
krbtgt
anakin
WS01-10$
WS02-7$
DC02$
han
POKEMON$
```



# Secrets


## NTLM Hash's
This is again the NT (or LM) Hash or Kerberos keys.

LM hashes are week and not used since 2008

NT hashes are a little stronger but don't use salt by default and thus can be cracked by something like rainbow tables.

The nt hash is generated like this:
`nt_hash = md4(encode_in_utf_16le(password))`


In case of LM is not being used, its value will be `aad3b435b51404eeaad3b435b51404ee`

Hash's are not passwords exactly but can be cracked or used in attacks like [[Pass the Hash]]


## Kerberos

The Kerberos keys can be used to ask for a Kerberos ticket that represents the user in Kerberos authentication. There are several different keys, and different ones are used for different Kerberos encryption support:

-   AES 256 key: Used by the [AES256-CTS-HMAC-SHA1-96](https://tools.ietf.org/html/rfc3962) algorithm. This is the one commonly used by Kerberos, and the one a pentester should use in order to avoid triggering alarms.
    
-   AES 128 key: Used by the [AES128-CTS-HMAC-SHA1-96](https://tools.ietf.org/html/rfc3962) algorithm.
    
-   DES key: Used by the [deprecated](https://datatracker.ietf.org/doc/html/rfc6649) [DES-CBC-MD5](https://datatracker.ietf.org/doc/html/rfc3961#section-6.2.1) algorithm.
    
-   RC4 key: This is the [NT hash](https://zer1t0.gitlab.io/posts/attacking_ad/#lm-nt-hashes) of the user used by the [RC4-HMAC](https://tools.ietf.org/html/rfc4757) algorithm.

These can be used in a [[pass the key]] attack




