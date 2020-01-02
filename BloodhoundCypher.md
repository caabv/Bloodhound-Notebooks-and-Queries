# Bloodhound Cypher

- [Bloodhound Cypher](#bloodhound-cypher)
  - [Bloodhound Queries](#bloodhound-queries)
    - [Find all Windows 7 computers](#find-all-windows-7-computers)
    - [Find unsupported (and potentially vulnerable) Windows OS](#find-unsupported-and-potentially-vulnerable-windows-os)
    - [If you want to enumerate all available properties](#if-you-want-to-enumerate-all-available-properties)
    - [Return labels i.e. group,username for a given SID](#return-labels-ie-groupusername-for-a-given-sid)
    - [Find every OU that contains the string "CITRIX"](#find-every-ou-that-contains-the-string-%22citrix%22)
    - [How many High Value Target a Node can reach](#how-many-high-value-target-a-node-can-reach)
    - [Get a count of computers that do not have admins](#get-a-count-of-computers-that-do-not-have-admins)
    - [Get the names of computers without admins, sorted in alphabetical order](#get-the-names-of-computers-without-admins-sorted-in-alphabetical-order)
    - [Return a list of users who have admin rights on at least one system either explicitly or through group membership](#return-a-list-of-users-who-have-admin-rights-on-at-least-one-system-either-explicitly-or-through-group-membership)
    - [Return username and number of computers that username is admin for, for top N users](#return-username-and-number-of-computers-that-username-is-admin-for-for-top-n-users)
    - [Show all users that are administrator on more than one machine](#show-all-users-that-are-administrator-on-more-than-one-machine)
    - [Show all users that are administrative on at least one machine, ranked by the number of machines they are admin on](#show-all-users-that-are-administrative-on-at-least-one-machine-ranked-by-the-number-of-machines-they-are-admin-on)
    - [Show computers where Domain Admins are logged in](#show-computers-where-domain-admins-are-logged-in)
    - [Show groups with most localAdmin](#show-groups-with-most-localadmin)
    - [Find WORKSTATIONS where Domain Users can RDP To](#find-workstations-where-domain-users-can-rdp-to)
    - [Find SERVERS where Domain Users can RDP To](#find-servers-where-domain-users-can-rdp-to)
    - [Computer where the most users can RDP to](#computer-where-the-most-users-can-rdp-to)
    - [Return users with shortest paths to high value targets by name](#return-users-with-shortest-paths-to-high-value-targets-by-name)
    - [Find Shortest Path from DOMAIN USERS to High Value Targets](#find-shortest-path-from-domain-users-to-high-value-targets)
    - [Shortest Path from Owned Principals](#shortest-path-from-owned-principals)
    - [Find ALL Path from DOMAIN USERS to High Value Targets](#find-all-path-from-domain-users-to-high-value-targets)
    - [Shortest Path from Domain Users to Domain Admins](#shortest-path-from-domain-users-to-domain-admins)
    - [Dodgiest path to DA or highvalue](#dodgiest-path-to-da-or-highvalue)
    - [Shortest Path from Domain Users to Domain Admins (No AdminTo)](#shortest-path-from-domain-users-to-domain-admins-no-adminto)
    - [DOMAIN USERS is Admin of Computer](#domain-users-is-admin-of-computer)
    - [Find all other right Domain Users shouldnt have](#find-all-other-right-domain-users-shouldnt-have)
    - [Too many admins on computer](#too-many-admins-on-computer)
    - [All DA Account Sessions](#all-da-account-sessions)
    - [DA Sessions to NON DC](#da-sessions-to-non-dc)
    - [Count on how many non DC machines DA have sessions](#count-on-how-many-non-dc-machines-da-have-sessions)
    - [Which non-DAs can control any DA and how](#which-non-das-can-control-any-da-and-how)
    - [Which non-DAs can control any GPO that applies to any DA and how](#which-non-das-can-control-any-gpo-that-applies-to-any-da-and-how)
    - [Extented - Explicit permissions or group delegated persmissions who control each GPO exclude DA](#extented---explicit-permissions-or-group-delegated-persmissions-who-control-each-gpo-exclude-da)
    - [This will return cross domain 'HasSession' relationships](#this-will-return-cross-domain-hassession-relationships)
    - [Cross domain attack paths](#cross-domain-attack-paths)
    - [Top 10 Computers with Most Admins](#top-10-computers-with-most-admins)
    - [Return top 10 users with most Derivative local admin rights](#return-top-10-users-with-most-derivative-local-admin-rights)
    - [Hunting password reuse between trusts](#hunting-password-reuse-between-trusts)
  - [Kerberos and delegation](#kerberos-and-delegation)
    - [Kerberoastable Accounts](#kerberoastable-accounts)
    - [Kerberoastable Accounts member of High Value Group](#kerberoastable-accounts-member-of-high-value-group)
    - [Count of kerberoastable users with path to eg. Domain Admin](#count-of-kerberoastable-users-with-path-to-eg-domain-admin)
    - [Most privileged kerberoastable users](#most-privileged-kerberoastable-users)
    - [List all computers with unconstraineddelegation](#list-all-computers-with-unconstraineddelegation)
    - [Shows machines that allow delegation that arent DCs](#shows-machines-that-allow-delegation-that-arent-dcs)
    - [Discover all unconstrained delegation systems that are not part of the domain controllers group](#discover-all-unconstrained-delegation-systems-that-are-not-part-of-the-domain-controllers-group)
      - [Tweak to previous - To mark all the aforementioned systems as high value](#tweak-to-previous---to-mark-all-the-aforementioned-systems-as-high-value)
  - [Nodes, Edges](#nodes-edges)
    - [Set SPN to a User](#set-spn-to-a-user)
    - [Add DOMAIN USERS as Admin to COMPxxxx](#add-domain-users-as-admin-to-compxxxx)
    - [Test our New Relationship (Try to replace AdminTo by 1**)](#test-our-new-relationship-try-to-replace-adminto-by-1)
    - [Find Admin Group not tagged highvalue](#find-admin-group-not-tagged-highvalue)
    - [Set Admin Group as highvalue](#set-admin-group-as-highvalue)
    - [Remove Admin User highvalue](#remove-admin-user-highvalue)
    - [EXCLUDE PATH (Edge) on the fly](#exclude-path-edge-on-the-fly)
    - [Delete a Relationship](#delete-a-relationship)
  - [Neo4j Console](#neo4j-console)
    - [Find every user object where the "userpassword" attribute is populated.](#find-every-user-object-where-the-%22userpassword%22-attribute-is-populated)
    - [Find any computer that is NOT a domain controller that is trusted to perform unconstrained delegation](#find-any-computer-that-is-not-a-domain-controller-that-is-trusted-to-perform-unconstrained-delegation)
    - [Find every user that doesnt require kerberos pre-authentication](#find-every-user-that-doesnt-require-kerberos-pre-authentication)
    - [Return any group where the name of the group contains the string "SQL" followed by the string "ADMIN". This will match "SQL ADMINS" and will also match "SQL 2017 ADMINS"](#return-any-group-where-the-name-of-the-group-contains-the-string-%22sql%22-followed-by-the-string-%22admin%22-this-will-match-%22sql-admins%22-and-will-also-match-%22sql-2017-admins%22)
    - [Find all computers with sessions from users of a different domain (Looking for cross-domain compromise opportunities)](#find-all-computers-with-sessions-from-users-of-a-different-domain-looking-for-cross-domain-compromise-opportunities)
    - [Find all users trusted to perform constrained delegation, return in order of the number of target computers](#find-all-users-trusted-to-perform-constrained-delegation-return-in-order-of-the-number-of-target-computers)
    - [Return each OU in the database in order of the number of computers in that OU](#return-each-ou-in-the-database-in-order-of-the-number-of-computers-in-that-ou)
    - [Return the name of every computer in the database where at least one SPN for the computer contains the string "MSSQL"](#return-the-name-of-every-computer-in-the-database-where-at-least-one-spn-for-the-computer-contains-the-string-%22mssql%22)
    - [Find groups with both users and computers that belong to the group](#find-groups-with-both-users-and-computers-that-belong-to-the-group)
    - [Return each OU in the database that contains a Windows Server computer. Return rows where the columns are the name of the OU, the name of the computer, and the operating system of the computer](#return-each-ou-in-the-database-that-contains-a-windows-server-computer-return-rows-where-the-columns-are-the-name-of-the-ou-the-name-of-the-computer-and-the-operating-system-of-the-computer)
    - [Find every instance of a computer account having local admin rights on other computers. Return in descending order of the number of computers the computer account has local admin rights on](#find-every-instance-of-a-computer-account-having-local-admin-rights-on-other-computers-return-in-descending-order-of-the-number-of-computers-the-computer-account-has-local-admin-rights-on)
  - [Bloodhound Reporting](#bloodhound-reporting)
    - [% of users with path to DA](#of-users-with-path-to-da)
    - [Stats percentage of enabled users that have a path to a high value group](#stats-percentage-of-enabled-users-that-have-a-path-to-a-high-value-group)
    - [Count how many Users have a path to DA](#count-how-many-users-have-a-path-to-da)
    - [DA Sessions to NON DC](#da-sessions-to-non-dc-1)
    - [Average Length of Path](#average-length-of-path)

## Bloodhound Queries

### Find all Windows 7 computers

```sql
MATCH (c:Computer)
WHERE toUpper(c.operatingsystem) CONTAINS "Windows 7"
RETURN c
```

### Find unsupported (and potentially vulnerable) Windows OS

```sql
MATCH (H:Computer) WHERE H.operatingsystem =~ '(?i).*(2000|2003|2008|xp|vista|me).*' RETURN H
```

### If you want to enumerate all available properties

```sql
Match (n:Computer) return properties(n)
```

### Return labels i.e. group,username for a given SID

```sql
MATCH (n {objectsid:"S-1-5-21-971234526-3761234589-3049876599-500"}) RETURN labels(n)
```

### Find every OU that contains the string "CITRIX"

```sql
MATCH (o:OU)
WHERE o.name =~ "(?i).*CITRIX.*"
RETURN o
```

### How many High Value Target a Node can reach

```sql
MATCH p = shortestPath((n)-[*1..]->(m {highvalue:true}))
WHERE NOT n = m
RETURN DISTINCT(m.name),LABELS(m)[0],COUNT(DISTINCT(n))
ORDER BY COUNT(DISTINCT(n)) DESC
```

### Get a count of computers that do not have admins

```sql
MATCH (n)-[r:AdminTo]->(c:Computer)
WITH COLLECT(c.name) as compsWithAdmins
MATCH (c2:Computer) WHERE NOT c2.name in compsWithAdmins
RETURN COUNT(c2)
```

### Get the names of computers without admins, sorted in alphabetical order

```sql
MATCH (n)-[r:AdminTo]->(c:Computer)
WITH COLLECT(c.name) as compsWithAdmins
MATCH (c2:Computer) WHERE NOT c2.name in compsWithAdmins
RETURN c2.name
ORDER BY c2.name ASC
```

### Return a list of users who have admin rights on at least one system either explicitly or through group membership

```sql
MATCH (u:User)-[r:AdminTo|MemberOf*1..]->(c:Computer
RETURN u.name
```

### Return username and number of computers that username is admin for, for top N users

```sql
MATCH (U:User)-[r:MemberOf|:AdminTo*1..]->(C:Computer)
WITH U.name as n, COUNT(DISTINCT(C)) as c 
RETURN n,c
ORDER BY c DESC LIMIT 5
```

### Show all users that are administrator on more than one machine

```sql
MATCH (U:User)-[r:MemberOf|:AdminTo*1..]->(C:Computer)
WITH U.name as n, COUNT(DISTINCT(C)) as c 
WHERE c>1
RETURN n
ORDER BY c DESC
```

### Show all users that are administrative on at least one machine, ranked by the number of machines they are admin on

```sql
MATCH (u:User)
WITH u
OPTIONAL MATCH (u)-[r:AdminTo]->(c:Computer)
WITH u,COUNT(c) as expAdmin
OPTIONAL MATCH (u)-[r:MemberOf*1..]->(g:Group)-[r2:AdminTo]->(c:Computer)
WHERE NOT (u)-[:AdminTo]->(c)
WITH u,expAdmin,COUNT(DISTINCT(c)) as unrolledAdmin
RETURN u.name,expAdmin,unrolledAdmin,expAdmin + unrolledAdmin as totalAdmin
ORDER BY totalAdmin ASC
```

### Show computers where Domain Admins are logged in

```sql
MATCH (n:User)-[:MemberOf]->(g:Group {name:"DOMAIN ADMINS@EXAMPLE.COM"})
WITH n as DAaccount
MATCH (c:Computer)-[b:MemberOf]->(t:Group) WHERE NOT t.name = "DOMAIN CONTROLLERS@EXAMPLE.COM"
WITH c as NonDC
MATCH p = (NonDC)-[:HasSession]->(DAaccount)
```

### Show groups with most localAdmin

```sql
MATCH (g:Group)
WITH g
OPTIONAL MATCH (g)-[r:AdminTo]->(c:Computer)
WITH g,COUNT(c) as expAdmin
OPTIONAL MATCH (g)-[r:MemberOf*1..]->(a:Group)-[r2:AdminTo]->(c:Computer)
WITH g,expAdmin,COUNT(DISTINCT(c)) as unrolledAdmin
RETURN g.name,expAdmin,unrolledAdmin, expAdmin + unrolledAdmin as totalAdmin
ORDER BY totalAdmin DESC 
```

### Find WORKSTATIONS where Domain Users can RDP To

```sql
match p=(g:Group)-[:CanRDP]->(c:Computer) where g.name STARTS WITH 'DOMAIN USERS' AND NOT c.operatingsystem CONTAINS 'Server' return p
```

### Find SERVERS where Domain Users can RDP To

```sql
match p=(g:Group)-[:CanRDP]->(c:Computer)
where g.name STARTS WITH 'DOMAIN USERS'
AND c.operatingsystem CONTAINS 'Server'
return p
```

### Computer where the most users can RDP to

```sql
MATCH (c:Computer)
OPTIONAL MATCH (u1:User)-[:CanRDP]->(c)
OPTIONAL MATCH (u2:User)-[:MemberOf*1..]->(:Group)-[:CanRDP]->(c)
WITH COLLECT(u1) + COLLECT(u2) as tempVar,c
UNWIND tempVar as users
RETURN c.name,COUNT(DISTINCT(users))
ORDER BY COUNT(DISTINCT(users)) DESC
```

### Return users with shortest paths to high value targets by name

```sql
MATCH (u:User)
MATCH (g:Group {highvalue: True})
MATCH p = shortestPath((u:User)-[r:AddMember|AdminTo|AllExtendedRights|AllowedToDelegate|CanRDP|Contains|ExecuteDCOM|ForceChangePassword|GenericAll|GenericWrite|GetChangesAll|GpLink|HasSession|MemberOf|Owns|ReadLAPSPassword|TrustedBy|WriteDacl|WriteOwner*1..]->(g))
RETURN DISTINCT(u.name),u.enabled
order by u.name
```

### Find Shortest Path from DOMAIN USERS to High Value Targets

```sql
MATCH (g:Group),(n {highvalue:true}),p=shortestPath((g)-[*1..]->(n))
WHERE g.name STARTS WITH 'DOMAIN USERS'
RETURN p
```

### Shortest Path from Owned Principals
<!-- [*1..3], 3 difines how many hops to be shown -->
```sql
MATCH p=shortestPath((c {owned: true})-[*1..3]->(s)) WHERE NOT c = s RETURN p
```

### Find ALL Path from DOMAIN USERS to High Value Targets

```sql
MATCH (g:Group) WHERE g.name STARTS WITH 'DOMAIN USERS'  MATCH (n {highvalue:true}),p=shortestPath((g)-[r*1..]->(n)) return p
```

### Shortest Path from Domain Users to Domain Admins

```sql
MATCH (g:Group {name:"DOMAIN USERS@TESTLAB.LOCAL"}),
(n {name:"DOMAIN ADMINS@TESTLAB.LOCAL"}),
p=shortestPath((g)-[r*1..]->(n))
return p
```

### Dodgiest path to DA or highvalue

```sql
MATCH p=shortestPath((u {highvalue: false})-[*1..]->(g:Group {name: 'DOMAIN ADMINS@HACKERS.LAB'}))
WHERE NOT (u)-[:MemberOf*1..]->(:Group {highvalue: true})
RETURN p
```

### Shortest Path from Domain Users to Domain Admins (No AdminTo)

```sql
MATCH (g:Group {name:"DOMAIN USERS@TESTLAB.LOCAL"}),
(n {name:"DOMAIN ADMINS@TESTLAB.LOCAL"}),
p=shortestPath((g)-[r*1..]->(n))
WHERE ALL(x in relationships(p) WHERE type(x) <> "AdminTo")
return p
```

### DOMAIN USERS is Admin of Computer

```sql
MATCH p=(m:Group)-[r:AdminTo]->(n:Computer) WHERE m.name STARTS WITH 'DOMAIN USERS' RETURN p
```

### Find all other right Domain Users shouldnt have

```sql
MATCH p=(m:Group)-[r:Owns|:WriteDacl|:GenericAll|:WriteOwner|:ExecuteDCOM|:GenericWrite|:AllowedToDelegate|:ForceChangePassword]->(n:Computer) WHERE m.name STARTS WITH 'DOMAIN USERS' RETURN p
```

### Too many admins on computer

```sql
MATCH (c:Computer {domain:'DOMAIN.COM'})
OPTIONAL MATCH (n1:User)-[:AdminTo]->(c)
OPTIONAL MATCH (n2:User)-[:MemberOf*1..]->(:Group)-[:AdminTo]->(c)
WITH COLLECT(n1) + COLLECT(n2) as tempVar,c
UNWIND tempVar as Admins
RETURN c.name,COUNT(DISTINCT(Admins)) as Admins
ORDER BY Admins DESC
```

### All DA Account Sessions

```sql
MATCH (n:User)-[:MemberOf]->(g:Group {name:"DOMAIN ADMINS@TESTLAB.LOCAL"})
MATCH p = (c:Computer)-[:HasSession]->(n)
return p
```

### DA Sessions to NON DC

```sql
OPTIONAL MATCH (c:Computer)-[:MemberOf]->(t:Group)
WHERE NOT t.name = "DOMAIN CONTROLLERS@TESTLAB.LOCAL"
WITH c as NonDC
MATCH p=(NonDC)-[:HasSession]->(n:User)-[:MemberOf]->
(g:Group {name:"DOMAIN ADMINS@TESTLAB.LOCAL"})
RETURN DISTINCT (n.name) as Username, COUNT(DISTINCT(NonDC)) as Connexions
ORDER BY COUNT(DISTINCT(NonDC)) DESC
```

### Count on how many non DC machines DA have sessions

```sql
MATCH (c:Computer)-[:MemberOf*1..]->(g:Group)
WHERE g.objectsid ENDS WITH "-516"
WITH c.name as DomainControllers
MATCH p = (c2:Computer)-[:HasSession]->(u:User)-[:MemberOf*1..]->(g:Group)
WHERE g.objectsid ENDS WITH "-512" AND NOT c2.name in DomainControllers
RETURN DISTINCT(u.name),COUNT(DISTINCT(c2))
ORDER BY COUNT(DISTINCT(c2)) DESC
```

### Which non-DAs can control any DA and how

```sql
MATCH p1 = (daPrincipal)-[:MemberOf*1..]->(daGroup:Group {name:'DOMAIN ADMINS@DOMAIN.COM'})
WITH p1,daPrincipal,daGroup
OPTIONAL MATCH p2 = (l)-[{isacl:true}]->(daPrincipal)
WHERE NOT l = daGroup AND NOT l = daPrincipal
OPTIONAL MATCH p3 = (m)-[:MemberOf*1..]->(g:Group)-[{isacl:true}]->(daPrincipal)
WHERE NOT m = daPrincipal AND NOT m = daGroup AND NOT g = daGroup AND NOT (m)-[:MemberOf*1..]->(daGroup)
RETURN p1,p2,p3
```

### Which non-DAs can control any GPO that applies to any DA and how

```sql
MATCH (g3:Group {name:'DOMAIN ADMINS@DOMAIN.COM'})
OPTIONAL MATCH p1 = (g1:GPO)-[:GpLink {enforced:false}]->(container)-[:Contains*1..]->(u1:User)-[:MemberOf*1..]->(g3)
WHERE NONE (x in NODES(p1) WHERE x.blocksinheritance = true AND x:OU AND NOT (g1)-->(x))
OPTIONAL MATCH p2 = (g2:GPO)-[:GpLink {enforced:true}]->(container)-[:Contains*1..]->(u2:User)-[:MemberOf*1..]->(g3)
RETURN p1,p2
```

### Extented - Explicit permissions or group delegated persmissions who control each GPO exclude DA

```sql
MATCH (g3:Group {name:'DOMAIN ADMINS@DOMAIN.COM'})
OPTIONAL MATCH p1 = (g1:GPO)-[:GpLink {enforced:false}]->(container)-[:Contains*1..]->(u1:User)-[:MemberOf*1..]->(g3)
WHERE NONE (x in NODES(p1) WHERE x.blocksinheritance = true AND x:OU AND NOT (g1)-->(x))
WITH p1,g1,g3,u1
OPTIONAL MATCH p2 = (g2:GPO)-[:GpLink {enforced:true}]->(container)-[:Contains*1..]->(u2:User)-[:MemberOf*1..]->(g3)
WITH p1,p2,g1,g2,g3,COLLECT(u1) + COLLECT(u2) as u
OPTIONAL MATCH p3 = (l)-[{isacl:true}]->(g1)
WHERE NOT l = g3
OPTIONAL MATCH p4 = (m)-[:MemberOf*1..]->(g4:Group)-[{isacl:true}]->(g1)
WHERE NOT (m)-[:MemberOf*1..]->(g3) AND NOT m = g3
OPTIONAL MATCH p5 = (n)-[{isacl:true}]->(g2)
WHERE NOT n = g3
OPTIONAL MATCH p6 = (o)-[:MemberOf*1..]->(g5:Group)-[{isacl:true}]->(g2)
WHERE NOT o IN u AND NOT o = g3 AND NOT g5 = g3
RETURN p1,p2,p3,p4,p5,p6
```

### This will return cross domain 'HasSession' relationships

```sql
MATCH p=((S:Computer)-[r:HasSession*1]->(T:User)) 
WHERE NOT S.domain = T.domain
RETURN p
```

### Cross domain attack paths

```sql
MATCH (c:Computer {domain:'DOMAIN.COM'})
OPTIONAL MATCH (n1:User)-[:AdminTo]->(c)
WHERE NOT n1.domain = c.domain
OPTIONAL MATCH (n2:User)-[:MemberOf*1..]->(:Group)-[:AdminTo]->(c)
WHERE NOT n2.domain = c.domain
WITH COLLECT(n1) + COLLECT(n2) as tempVar,c
UNWIND tempVar as foreignAdmins
RETURN c.name,COUNT(DISTINCT(foreignAdmins)) as foreignAdmins
ORDER BY foreignAdmins DESC
```

### Top 10 Computers with Most Admins

```sql
MATCH (n:User),(m:Computer), (n)-[r:AdminTo]->(m) WHERE NOT n.name STARTS WITH 'ANONYMOUS LOGON' AND NOT n.name='' WITH m, count(r) as rel_count order by rel_count desc LIMIT 10 MATCH p=(m)<-[r:AdminTo]-(n) RETURN p
```

### Return top 10 users with most Derivative local admin rights

```sql
MATCH (u:User)
OPTIONAL MATCH (u)-[:AdminTo]->(c1:Computer)
OPTIONAL MATCH (u)-[:MemberOf*1..]->(:Group)-[:AdminTo]->(c2:Computer)
WITH COLLECT(c1) + COLLECT(c2) as tempVar,u
UNWIND tempVar AS computers
RETURN u.name,COUNT(DISTINCT(computers)) AS is_admin_on_this_many_boxes
ORDER BY is_admin_on_this_many_boxes DESC
```

### Hunting password reuse between trusts

```sql
MATCH (u1:User {domain: "(link: http://DOMAIN1.COM) DOMAIN1.COM"}) MATCH (u2:User {domain: "(link: http://DOMAIN2.COM) DOMAIN2.COM"}) WHERE (toLower(split((link: http://u1.name) u1.name, "@")[0]) = toLower(split((link: http://u2.name) u2.name, "@")[0])) return u1, u2
```

## Kerberos and delegation

### Kerberoastable Accounts

```sql
MATCH (n:User)WHERE n.hasspn=true RETURN n
```

### Kerberoastable Accounts member of High Value Group

```sql
MATCH (n:User)-[r:MemberOf]->(g:Group) WHERE g.highvalue=true AND n.hasspn=true RETURN n, g, r
```

### Count of kerberoastable users with path to eg. Domain Admin

```sql
MATCH (u:User {hasspn:true})
MATCH (g:Group {name:'DOMAIN ADMINS@CONTOSO.LOCAL'})
MATCH p = shortestPath(
  (u)-[*1..]->(g)
)
RETURN u.name,LENGTH(p)
ORDER BY LENGTH(p) ASC
```

### Most privileged kerberoastable users

```sql
MATCH (u:User {hasspn:true})
OPTIONAL MATCH (u)-[:AdminTo]->(c1:Computer)
OPTIONAL MATCH
(u)-[:MemberOf*1..]->(:Group)-[:AdminTo]->(c2:Computer)
WITH u,COLLECT(c1) + COLLECT(c2) AS tempVar
UNWIND tempVar AS comps
RETURN u.name,COUNT(DISTINCT(comps))
ORDER BY COUNT(DISTINCT(comps)) DESC
```

### List all computers with unconstraineddelegation

```sql
MATCH (c:Computer {unconstraineddelegation:true})
return c.name
```
### Shows machines that allow delegation that arent DCs

```sql
OPTIONAL MATCH (c:Computer)-[:MemberOf]->(t:Group)  
WHERE NOT t.name =~ "(?i)DOMAIN CONTROLLERS*."  
WITH c as NonDC  
MATCH (NonDC {unconstraineddelegation:true}) RETURN NonDC.name
```

### Discover all unconstrained delegation systems that are not part of the domain controllers group

```sql
MATCH (dc:Computer)-[:MemberOf*1..]->(g:Group)
WHERE g.objectsid ENDS WITH "516"
WITH COLLECT(dc) as domainControllers
MATCH p = (d:Domain)-[:Contains*1..]->(c:Computer {unconstraineddelegation:true})
WHERE NOT c in domainControllers
RETURN c
```

#### Tweak to previous - To mark all the aforementioned systems as high value

```sql
MATCH (dc:Computer)-[:MemberOf*1..]->(g:Group)
WHERE g.objectsid ENDS WITH "516"
WITH COLLECT(dc) as domainControllers
MATCH p = (d:Domain)-[:Contains*1..]->(c:Computer {unconstraineddelegation:true})
WHERE NOT c in domainControllers
SET c.highvalue = true
RETURN c
```

## Nodes, Edges

### Set SPN to a User

```sql
MATCH (n:User {name:"JNOA00093@TESTLAB.LOCAL"}) SET n.hasspn=true
```

### Add DOMAIN USERS as Admin to COMPxxxx

```sql
MERGE (n:Group {name:"DOMAIN USERS@TESTLAB.LOCAL"}) WITH n MERGE (m:Computer {name:"COMP00673.TESTLAB.LOCAL"}) WITH n,m MERGE (n)-[:AdminTo]->(m)
```

### Test our New Relationship (Try to replace AdminTo by 1**)

```sql
MATCH p=(n:Group {name:"DOMAIN USERS@TESTLAB.LOCAL"})-[r:AdminTo]->(m:Computer {name:"COMP00673.TESTLAB.LOCAL"})
RETURN p
```

### Find Admin Group not tagged highvalue

```sql
MATCH (g:Group)
WHERE g.name CONTAINS "ADMIN"
AND g.highvalue IS null
RETURN g.name
```

### Set Admin Group as highvalue

```sql
MATCH (g:Group)
WHERE g.name CONTAINS "ADMIN"
AND g.highvalue IS null
SET g.highvalue=true
```

### Remove Admin User highvalue

```sql
MATCH p=(g:Group {highvalue: true})<-[:MemberOf]-(u:User)
WHERE g.name CONTAINS "ADMIN"
SET g.highvalue = NULL
```

### EXCLUDE PATH (Edge) on the fly

```sql
MATCH (n:User),(m:Group {name:"DOMAIN ADMINS@TESTLAB.LOCAL"}),
p=shortestPath((n)-[r*1..]->(m))
WHERE ALL(x in relationships(p) WHERE type(x) <> "AdminTo")
RETURN p
```

### Delete a Relationship

```sql
MATCH p=(n:Group {name:"DOMAIN USERS@TESTLAB.LOCAL"})-[r:AdminTo]->
(m:Computer {name:"COMP00673.TESTLAB.LOCAL"})
DELETE r
```

## Neo4j Console

### Find every user object where the "userpassword" attribute is populated. 

```sql
MATCH (u:User)
WHERE NOT u.userpassword IS null
RETURN u.name,u.userpassword
```

### Find any computer that is NOT a domain controller that is trusted to perform unconstrained delegation

```sql
MATCH (c1:Computer)-[:MemberOf*1..]->(g:Group)
WHERE g.objectsid ENDS WITH "-516"
WITH COLLECT(c1.name) AS domainControllers
MATCH (c2:Computer {unconstraineddelegation:true})
WHERE NOT c2.name IN domainControllers
RETURN c2.name,c2.operatingsystem
ORDER BY c2.name ASC
```

### Find every user that doesnt require kerberos pre-authentication

```sql
MATCH (u:User {dontreqpreauth: true})
RETURN u.name
```

### Return any group where the name of the group contains the string "SQL" followed by the string "ADMIN". This will match "SQL ADMINS" and will also match "SQL 2017 ADMINS"

```sql
MATCH (g:Group)
WHERE g.name =~ '(?i).*SQL.*ADMIN.*'
RETURN g.name
```

### Find all computers with sessions from users of a different domain (Looking for cross-domain compromise opportunities)

```sql
MATCH (c:Computer)-[:HasSession]->(u:User {domain:'FOO.BAR.COM'})
WHERE NOT c.domain = u.domain
RETURN u.name,COUNT(c)
```

### Find all users trusted to perform constrained delegation, return in order of the number of target computers

```sql
MATCH (u:User)-[:AllowedToDelegate]->(c:Computer)
RETURN u.name,COUNT(c)
ORDER BY COUNT(c) DESC
```

### Return each OU in the database in order of the number of computers in that OU

```sql
MATCH (o:OU)-[:Contains]->(c:Computer)
RETURN o.name,o.guid,COUNT(c)
ORDER BY COUNT(c) DESC
```

### Return the name of every computer in the database where at least one SPN for the computer contains the string "MSSQL"

```sql
MATCH (c:Computer)
WHERE ANY (x IN c.serviceprincipalnames WHERE toUpper(x) CONTAINS "MSSQL")
RETURN c.name,c.serviceprincipalnames
ORDER BY c.name ASC
```

### Find groups with both users and computers that belong to the group

```sql
MATCH (c:Computer)-[r:MemberOf*1..]->(groupsWithComps:Group)
WITH groupsWithComps
MATCH (u:User)-[r:MemberOf*1..]->(groupsWithComps)
RETURN DISTINCT(groupsWithComps) as groupsWithCompsAndUsers
```

### Return each OU in the database that contains a Windows Server computer. Return rows where the columns are the name of the OU, the name of the computer, and the operating system of the computer

```sql
MATCH (o:OU)-[:Contains]->(c:Computer)
WHERE toUpper(o.name) STARTS WITH "WINDOWS SERVER"
RETURN o.name,c.name,c.operatingsystem
```

### Find every instance of a computer account having local admin rights on other computers. Return in descending order of the number of computers the computer account has local admin rights on

```sql
MATCH (c1:Computer)
OPTIONAL MATCH (c1)-[:AdminTo]->(c2:Computer)
OPTIONAL MATCH (c1)-[:MemberOf*1..]->(:Group)-[:AdminTo]->(c3:Computer)
WITH COLLECT(c2) + COLLECT(c3) AS tempVar,c1
UNWIND tempVar AS computers
RETURN c1.name,COUNT(DISTINCT(computers))
ORDER BY COUNT(DISTINCT(computers)) DESC
```

## Bloodhound Reporting

### % of users with path to DA

```sql
OPTIONAL MATCH p=shortestPath((u:User)-[*1..]->
(m:Group {name: "DOMAIN ADMINS@TESTLAB.LOCAL"}))
OPTIONAL MATCH (uT:User)
WITH COUNT (DISTINCT(uT)) as uTotal,
COUNT (DISTINCT(u)) as uHasPath
RETURN uHasPath / uTotal * 100 as Percent
```

### Stats percentage of enabled users that have a path to a high value group

```
MATCH (u:User {domain:'EXAMPLE.COM',enabled:True})
MATCH (g:Group {domain:'EXAMPLE.COM'})
WHERE g.highvalue = True
WITH g, COUNT(u) as userCount
MATCH p = shortestPath((u:User {domain:'EXAMPLE.COM',enabled:True})-[*1..]->(g))
RETURN toint(100.0 * COUNT(distinct u) / userCount)
```

```sql
# MATCH (u:User)
# MATCH (g:Group {highvalue: True})
# MATCH p = shortestPath((u:User)-[r:AddMember|AdminTo|AllExtendedRights|AllowedToDelegate|CanRDP|Contains|ExecuteDCOM|ForceChangePassword|GenericAll|GenericWrite|GetChangesAll|GpLink|HasSession|MemberOf|Owns|ReadLAPSPassword|TrustedBy|WriteDacl|WriteOwner*1..]->(g))
# RETURN DISTINCT(u.name),u.enabled
# order by u.name
```

### Count how many Users have a path to DA

```sql
MATCH p=shortestPath((u:User)-[*1..]->
(m:Group {name: "DOMAIN ADMINS@TESTLAB.LOCAL"}))
RETURN COUNT (DISTINCT(u))
```

### DA Sessions to NON DC

```sql
OPTIONAL MATCH (c:Computer)-[:MemberOf]->(t:Group)
WHERE NOT t.name = "DOMAIN CONTROLLERS@TESTLAB.LOCAL" WITH c as NonDC
MATCH p=(NonDC)-[:HasSession]->(n:User)-[:MemberOf]->
(g:Group {name:"DOMAIN ADMINS@TESTLAB.LOCAL"})
RETURN DISTINCT (n.name) as Username, COUNT(DISTINCT(NonDC)) as Connections
ORDER BY COUNT(DISTINCT(NonDC)) DESC
```

```sql
OPTIONAL MATCH (c:Computer)-[:MemberOf]->(t:Group)
WHERE NOT t.name = "DOMAIN CONTROLLERS@TESTLAB.LOCAL"
WITH c as NonDC
MATCH p=(NonDC)-[:HasSession]->(n:User)-[:MemberOf]->
(g:Group {name:"DOMAIN ADMINS@TESTLAB.LOCAL"})
RETURN DISTINCT ([n.name,n.displayname]),
COUNT(DISTINCT(NonDC)) as Connections
ORDER BY COUNT(DISTINCT(NonDC)) DESC
```

### Average Length of Path

```sql
MATCH (g:Group {name:'DOMAIN ADMINS@CONTOSO.LOCAL'})
MATCH p = shortestPath((u:User)-[r:AdminTo|MemberOf|HasSession*1..]->(g))
WITH EXTRACT(n in NODES(p) | LABELS(n)[0]) as pathNodes
WITH FILTER(x IN pathNodes WHERE x = "Computer") as filteredPathNodes
RETURN AVG(LENGTH(filteredPathNodes))
```
