---
title: "PowerShell Comands"
permalink: /oscp/ps_scripts/
excerpt: "PS scripts for enumeration and creation of AD objects"
last_modified_at: 
toc: true
toc_sticky: true
---



title: dnsutils
nslookup is a DNS client that can be used to query SRV records. It usually comes with the dnsutils package.

```bash
# find the PDC (Principal Domain Controller)
nslookup -type=srv _ldap._tcp.pdc._msdcs.$FQDN_DOMAIN

# find the DCs (Domain Controllers)
nslookup -type=srv _ldap._tcp.dc._msdcs.$FQDN_DOMAIN

# find the GC (Global Catalog, i.e. DC with extended data)
nslookup -type=srv gc._msdcs.$FQDN_DOMAIN

# Other ways to find services hosts that may be DCs 
nslookup -type=srv _kerberos._tcp.$FQDN_DOMAIN
nslookup -type=srv _kpasswd._tcp.$FQDN_DOMAIN
nslookup -type=srv _ldap._tcp.$FQDN_DOMAIN
```






```bash
nmap --script dns-srv-enum --script-args dns-srv-enum.domain=$FQDN_DOMAIN
```


```bash
nmap -v -sV -p 53 $SUBNET/$MASK
nmap -v -sV -sU -p 53 $SUBNET/$MASK
```


```bash
# standard lookup
host $hostname

# reverse lookup
host $IP_address

# manual PTR resolution request
nslookup -type=ptr $IP_address
```

# Users
```powershell
#To get general info on user
PS C:\> Get-ADUser AccountName

#Enumerate Users
PS C:\> Get-ADUser -Filter * | select SamAccountName

#For a more complete list
PS C:\> Get-ADObject -LDAPFilter "objectClass=User" -Properties SamAccountName | select SamAccountName
```

# Domain

```powershell
#To get SID
PS C:\> Get-ADDomain | select DNSRoot,NetBIOSName,DomainSID
```

# Forest

```powershell
#To get the forest
PS C:\>Get-ADForest

#forest mode
PS C:\Users\Administrator\Downloads> (Get-ADForest).ForestMode
#Domain Mode
PS C:\Users\Administrator\Downloads> (Get-ADDomain).DomainMode

```

# Trusts

```powershell
#list of trusts
PS C:\Users\Administrator> nltest /domain_trusts
```