# 6. Route53

- Route53 is Amazon's DNS Service
- It's named after port 53 which is the DNS port
- Route53 is a global service (no region)

## 6.1 DNS

- DNSs convert domain names to IP Addresses. 
- IP addresses: IPv4 (32-bit), IPv6 (128-bit)
- IP addresses top-level domains controlled by IANA (Internet Assigned Numbers Authority). Official top level domains are listed on their [website](http://www.iana.org/domains/root/db).
- Domain registrars are authorities that can assign domain names directly under one or more top-level domain name.
- Domain registrars register names with InterNIC that ensures domain names are unique.

## 6.2 How DNSs work?

- User requests website e.g. Google.com
- ISP will check if IP of google.com is cached. If yes, send packets to IP.
- Otherwise do an `nslookup` of top level domain `.com`. The Name server will return an NS record:

domains 	| 	ttl 	| 	name server
--------------------------------------------
google.com 	| 	172800 	| 	ns.awsdns.com

- ISP will request IP of google.com from `ns.awsdns.com`.
- `ns.awsdns.com` will have the start of authority record.

## 6.3 SOA Records

- A Start of Authority (SOA) resource record indicates which domain name server (DNS) is the best source of information for the specified domain. 
- Every domain must have an SOA record.

An SOA Record contains

- The primary name server for the domain
- The email for the domain administrator, hostmaster@rackspace.com
- Minimum TTL, which is the default that applies to all resource records in the zone

## 6.3 DNS Record

DNS records contain: 
- **A name** (Address record): maps domain name to an ip address e.g.`google.com -> 80.80.80.80`.
- **C name** (Canonical name): Maps a domain name to an exising domain name e.g. `mobile.google.com -> m.google.com`
- **Alias records** (specific to AWS): maps a domain name to an AWS resource e.g. s3 website or ELB (e.g. map `www.mywebsite.com` to `timothy1.s3-website.eu-west-1.awsamazon.com`)

**Note:**

1.  `A` names must resolve to an IP address. `C name` and `Alias` records must resolve to another domain.
2. `C names` can **NOT** be naked domain names e.g. `http://google.com` is not a valid C Name. `http://m.google.com` and `http://www.google.com` are valid CNames
3. `Alias` names were introduced by amazon to allow naked domain names to point to amazon resources. They overcome the limitation of C name.
4. ELBs do not have fixed IPv4 addresses. They are resolved by the DNS using alias records. WHhen the IP address of a load balancer changes, the DNS record is updated automatically

## 6.4 Routing Types

### 6.5.1 Simple Routing Policy

- Only allows one A-record
- A-record may contain one or more IP addresses.
- When an IP address is requested by an IP from a simple-routing DNS and more than one IP address is listed, it will randomly pick one address.

### 6.5.2 Weighted Routing Policy

- Allows multiple record sets
- Weights can be assigned to each record set.
- e.g. 80% of traffic goes to `us-west-1` and 20% goes to `us-east-2`

### 6.5.3 Latency Routing Policy

- Routes traffic based on which record set will give the user the shortest latency
- An EC2/ELB instance is created in multiple regions
- A latency record set is then created for each region
- One latency record set can contain more than one EC2 ip addresses for the same region.

### 6.5.4 Failover Routing Policy

- Active/Passive set-up
- A record set is created for a primary instance and for a secondary instance
- e.g. Primary site is `us-west-1` and secondary site is `us-east-2`
- When there is an outage in the primary site, the traffic is redirected to the passive site.

### 6.5.4 Geolocation Routing Policy

- Routing is configured such that traffic from a particular region is routed to a particular instance.
- e.g. All European traffic is routed to eu-west-1 and all US traffic is routed to `us-west-1`.
- Geolocation is configured manually, the instance is not decided based on latency

### 6.5.6 Multivalue Routing Policy

- Allows multiple record sets, each with a different IP
- If more than one Ip exists, one is chosen at random
- If an instance health check fails, traffic is rerouted to another IP


**Questions**

1. Route53 is Amazon's DNS Service.

- [ ] A. Correct
- [ ] B. Incorrect

2. Does Route 53 support MX Records?

- [ ] A. Yes
- [ ] B. No
- [ ] C. Only in `us-west-1`
- [ ] D. Only if they have a SPF record

3. What is the significance of the `53` in `Route53`?

4. Route53 does not support zone apex records (or naked domain names)

- [ ] A. Correct
- [ ] B. Incorrect
- [ ] C. Depends on the circumstances
- [ ] D. Only on US-East 1

5. Which of the following is a valid A name entry 

- [ ] A. www.google.com
- [ ] B. `www.timothy.com` -> `timothy1.s3-website.eu-west-1.awsamazon.com`
- [ ] C. `small.ml` -> `52.36.254.31`
- [ ] D. `m.dudagdtest.com`->`mobile.dudagdtest.com`

6. Which of the following is a valid C name entry 

- [ ] A. www.google.com
- [ ] B. `www.timothy.com` -> `timothy1.s3-website.eu-west-1.awsamazon.com`
- [ ] C. `small.ml` -> `52.36.254.31`
- [ ] D. `m.dudagdtest.com`->`mobile.dudagdtest.com`

7. Which of the following is a valid Alias name entry 

- [ ] A. www.google.com
- [ ] B. `www.timothy.com` -> `timothy1.s3-website.eu-west-1.awsamazon.com`
- [ ] C. `small.ml` -> `52.36.254.31`
- [ ] D. `m.dudagdtest.com`->`mobile.dudagdtest.com`

8. Explain the difference between C names and alias names.

9. Is there is a limit to the number of domain names that you can manage using Route 53.

- [ ] A. 10 domain names, but more can be requested
- [ ] B. There is no limit
- [ ] C. 10 domain names, but more can not be requested

10. Every domain has a start of authority record. State the purpose of this record?

11. Which of these is NOT present in a SOA

- [ ] A. The DNS that is the best source of information for a domain
- [ ] B. The administrator of the domain
- [ ] C. The IP address of the domain
- [ ] D. The TTL of the SOA record

12. Why is a domain registered using the dns of a load balancer instead of the its IP address

13. An international news company hosts instances of its website in Australia, Europe, USA, South America and in Mumbai. Each instance focuses on news that is most relevant to its region. The news company would like that its users are routed to the instance that is closest to them so that their customers read news that is nost relavant to them. Which Route53 Routing policy could they use:

- [ ] A. Simple Routing Policy
- [ ] B. Weighted Routing Policy
- [ ] C. Latency Routing Policy
- [ ] D. Failover Routing Policy
- [ ] E. Geolocation Routing Policy
- [ ] F. Multivalue Routing Policy

14. A government security website must always be available as it is used for security at airports. If the website were to go offline, it could cause delays. Two instances of the website are set up in the US region: `us-east-1 and `us-west-1. The `us-east-1` is closest to the offices of government and is updated more frequently. It is preferred that requests are sent to the `us-east-1` primarly, and to the `us-west-1` only if the `us-east-1` instance isn't available. Which routing policy is most suited for this?

- [ ] A. Simple Routing Policy
- [ ] B. Weighted Routing Policy
- [ ] C. Latency Routing Policy
- [ ] D. Failover Routing Policy
- [ ] E. Geolocation Routing Policy
- [ ] F. Multivalue Routing Policy

15. `SportsView` is a world-renowned website that allows users to stream sports from all around the world. Their website has load-balanced instances in every AWS region around the world.  What routing policy would best suite `SportsView` to provide the smoothest streaming experience to all of its users?

- [ ] A. Simple Routing Policy
- [ ] B. Weighted Routing Policy
- [ ] C. Latency Routing Policy
- [ ] D. Failover Routing Policy
- [ ] E. Geolocation Routing Policy
- [ ] F. Multivalue Routing Policy

16. A popular online restaurant has several instances of its website hosted on AWS in one region. It simply wants the traffic to be evenly distributed between those instances while guaranteeing that the website is always available.

- [ ] A. Simple Routing Policy
- [ ] B. Weighted Routing Policy
- [ ] C. Latency Routing Policy
- [ ] D. Failover Routing Policy
- [ ] E. Geolocation Routing Policy
- [ ] F. Multivalue Routing Policy

17. A company has completed a major overhaul of its website. This overhaul has been termed version 2. The company would like to do a phased rollout of the new website. Initially, they would like 5% of their users to be redirected to Version 2 and the rest will be redirected to version 1. Eventually 0% traffic will be sent to version 1. Which routing policy is best suited for this?

- [ ] A. Simple Routing Policy
- [ ] B. Weighted Routing Policy
- [ ] C. Latency Routing Policy
- [ ] D. Failover Routing Policy
- [ ] E. Geolocation Routing Policy
- [ ] F. Multivalue Routing Policy

18. Which values are correct so that 5% of traffic is routed to instance a and 95% of traffic is routed to instance b.

- [ ] A. 5,95
- [ ] B. 5, 100

19. Timothy hosts a website on 2 EC2 instances in Europe. Why 2? God knows! Probably was born into a rich snobby family. Anyway, because Timothy is a spoilt brat, no body visits his website. What routing policy would return all available records for his website in any order (no health checks needed)

- [ ] A. Simple Routing Policy
- [ ] B. Weighted Routing Policy
- [ ] C. Latency Routing Policy
- [ ] D. Failover Routing Policy
- [ ] E. Geolocation Routing Policy
- [ ] F. Multivalue Routing Policy

Answers
1. A. Duh! 
2. A. Route53 supports MX records.
3. 53 is the port on which DNSs operate
4. B. Alias was introduced by AWS to add support for naked domain names.
5. C. `A` names must map to an ip address
6. D. 
7. B. `Alias` is AWS specific. It is used to map domain names to AWS services such as S3 websites.
8. C names do not support mapping naked domains e.g. `google.com`, whereas alias names do.
9. A. There is a soft limit of 10 domain names in Route 53
10. The start of authority states which DNS is the best authority for a domain.
11. C. The Start of Authority record does not contain the IP of the domain
12. ELBs do not have a fixed IP address. A DNS address is used because when the IP address of an ELB changes, the A record in the ELB's DNS will automatically be updated.
13. E (Geolocation routing policy). The Geolocation routing policy allows the company to manually configure that all traffic from a particular region is routed to a particular IP Address.
14. D (Failover routing policy). This Failover routing policy allows one instance to be set up as the primary instance and another instance to be set up as the secondary instance. The traffic is routed to the secondary instance when the primary instance is unresponsive or offline.
15. C (Latency Routing Policy). Latency routing policy routes traffic to the instance that will provide the user with the lowest latency. Low latency is important for smooth streaming.
16. F (Multivalue Routing Policy). Multivalue routing policy allows multiple DNS records to be recorded. When the DNS is queried, one of the records is returned at random. If one of the DNS records is unhealthy, it will not be used.
17. B. Weighted routed policy allows the overall traffic to be divided into proportions where each portion is routed to a different instance according.
18. A.
19. A. Simple Routing Policy. Only allows one A record but multiple IPs can be listed in that record. When the DNS is queried all records are returned at once.