# An Introduction to Application Security

## Key terms, standards and organizations

* CWE - Common Weakness Enumeration
	* Global community developed dictionary of software weakness types which provides a standard, unified, and measurable set of software weaknesses

# An Introduction to Encrypting Passwords in Config Files

## How to encrypt plaintext passwords - ASP.NET

The .NET Framework includes two protected configuration providers to protect web.config elements:

* **DPAPIProtectionConfigurationProvider** - Uses the Windows Protection API to encrypt/decrypt data. Supports Machine and User Stores for key storage.
* **RsaProtectedConfigurationProvider** - Uses RSA Encryption to encrypt/decrypt data

Use Machine Store if your application runs on it's own dedicated server or multiple applications on this server share sensitive information.

Use User Store if your application runs on a shared hosting environment and your sensitive data is not accessible to other applications, each application should run under a different identity and the protected resources should be restricted to this identity. In that case each application should run under a separate application pool identity and the resources should be restricted to that identity. 

### Demo

#### DataProtectionConfigurationProvider

Execute the following to encrypt: **aspnet_regiis -pe connectionStrings -app /DPAPITest -prov DataProtectionConfigurationProvider**

* -pe specifies which element should be encrypted
* -app specifies the virtual application path

You will receive a "Encrypting configuration section... Succeeded!" message if it worked.

Execute the following to decrypt: **aspnet_regiis -pd connectionStrings -app/DPAPITest**

Using aspnet_regiis to protect the configuration element is a good choice because:

* simple to deploy
* No 3rd party libs required
* No need to change source code
* The .NET Framework will decrypt the data transparently for your app
* With DPAPI the Key Management is handled by Windows

# An Introduction to Insecure Direct Object Reference

This occurs when the application fails to prevent the user from accessing another users' data by manipulating the resource identifier to the data in the user request.

Ex. providing userId in the querystring, freely editable.

### Approaches to prevent Insecure Direct Object References

1. Use per user or session indirect object reference
	1. Application handles the mapping between user and the actual backend data, ensures account ID can't be tampered with.
2. Perform Authorization check
	1. For all the data that has a direct object reference, applications should perform an access check to ensure user is authorized to access the requested data.
3. Use cryptographic randomization to make it difficult to guess the correct key
	1. .NET - System.Security.Cryptography.RNGCryptoServiceProvider

### How to Test for Insecure Direct Object Reference

* Identity request parameters that reference restricted data.
* Develop misuse tests to manipulate parameter values to ensure unauthorized access is detected and is unsuccessful
* A SSLProxy (ex. Fiddler2) can be used to intercept and modify requests sent to the application
* NOTE: Automated tools typically do not look for such flaws because they cannot recognize what requires protection or what is safe or unsafe. 

# Open Source Policy

In general: Apache, BSD, MIT and LGPL can be used without concern. GPL assumes some moderate risk, but may be okay for applications that are internal-only and will never be distributed. Probably best to verify before using a GPL licensed library. 

Use of an OS library will sometimes require you to inform Legal, and in some cases be audited. 

* OSSAT (Open Source Screening and Assessment Tool) - internal web application that allows developers to notify Legal when they use OSS components, or to request approval to use those components. 
* Protex - third party app that scans source code and discovers potential uses of OSS. For audit of unapproved components. 

## Four Software Classes

### 1) Tools and Infrastructure 

Ex. Eclipse/Cygwin, Subversion, MySQL, etc.

Inform | Request | Audit
-------|---------|-------
No | No | No

### 2) Internal

Software resident on systems managed by Thomson Reuters, but never directly accessed by customers (home-grown CMS, internal monitoring, etc)

Inform | Request | Audit
-------|---------|-------
Yes | No | No

### 3) Hosted
Software hosted on systems owned or controlled by Thomson Reuters, which are direct accessed by customers. 

Inform | Request | Audit
-------|---------|-------
Yes | AGPL Only | No

### 4) Distributed

Software installed on hardware neither owned nor controlled by Thomson Reuters.

Inform | Request | Audit
-------|---------|-------
Yes | Yes | Yes

### What about the cloud?

By default, the Class 4 rules apply to all software hosted outside of Thomson Reuters data centers. 

However, if Legal explicitly reviews and approves a third party hosting agreement, the Class 3 rules may apply instead. 

## Your Role

* When adding open source to a project:
	* Class 2, 3, and 4: immediately enter it into OSSAT
* Before any customer-facing releases: scan Class 4 with Protex and verify:
	* All open source code deteced has been approved in OSSAT
* Build sufficient time into your development process to allow for compliance with this policy, and remediation of any items if necessary.
* Consult legal if you are ever uncertain. 