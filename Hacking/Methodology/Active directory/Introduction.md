Active directory is a phonebook of objects such as printers, computers and else.
Very large number of internal networks use AD, so knowing how to pentest AD's is great.

## Components

## Physical AD Components

`Domain controller` - is the most important server in AD
It has capability to manage authentication, authorization, network resources etc.
It hosts a copy of the AD DS directory store.

Compromise of the DC means compromise of the whole network.

`AD DS Data Store`
Contains  the database files and processes that manage directory info for users, services and apps.

- Consists of the Ntds.dit file
- I stored by default in the %SystemRoot%\NTDS folder on all DC's
- Is only accessible thru the domain controller processes and protocols

## Logical AD Components

`AD DS Schema` -  Enforces rules of objects creation.

- Defines every type of object  that can be stored in the directory
- Enforces rules regarding object creation and configuration

 Object types                                    Function                                                    Examples 

Class Object           What objects can be created in the directory    User, Computer

Attribute Object   Information that can be attached to an obj     Display name

`Domains` - are used to group and manage objects in an organization.

- An administrative boundary for applying policies to groups of objects
- A replication policy for replicating data between domain controllers
- An authentication and authorization boundary that provides a way to limit the scope of access to resources

`Trees` - Group of domains

![[Pasted image 20241118145948.png]]

All domains in the tree:
- Share a contiguous namespace with the parent domain
- Can have additional child domains
- By default create a two-way transitive trust with other domains

`Forests` - Group of domain trees
![[Pasted image 20241118172016.png]]

- Share a common schema
- Share a common configuration partition
- Share a common global catalog to enable searching
- Enable trusts between all domains in the forest
- Share the Enterprise Admins and Schema Admins group

Inside the AD there are `Organizational Units (Ous)`
Ous are AD containers
![[Pasted image 20241118172316.png]]

They are used to:
- Represent your org hierarchically and logically
- Manage a collection of objects in a consistent way
- Delegate  permissions to administer groups of objects
- Apply policies

`Trusts`
Provide a mechanism for users to gain access to resources in another domain

![[Pasted image 20241118172509.png]]

`Objects`
Are inside the Ous

![[Pasted image 20241118172643.png]]

