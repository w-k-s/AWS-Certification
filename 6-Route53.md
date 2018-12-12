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
- Otherwise do an ns lookup of top level domain `.com`. The Name server will return an NS record:

domains 	| 	ttl 	| 	name server
--------------------------------------------
google.com 	| 	172800 	| 	ns.awsdns.com

- ISP will request IP of google.com from `ns.awsdns.com`.
- `ns.awsdns.com` will have the strat of authority record

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
- **C name** (Canonical name): Maps a domain name to an exising domain name e.g. `mobile.google.com -> m.google.com`.
- **Alias records** (specific to AWS): maps a domain name to an AWS resource e.g. s3 website or ELB (e.g. map `www.mywebsite.com` to `timothy1.s3-website.eu-west-1.awsamazon.com`)

**Note:**

1.  `A` names must resolve to an IP address. `C name` and `Alias` records must resolve to another domain.
2. `C names` can **NOT** be naked domain names e.g. `http://google.com` is not a valid C Name. `http://m.google.com` and `http://www.google.com` are valid CNames
3. `Alias` names were introduced by amazon to allow naked domain names to point to amazon resources. They overcoem the limitation of C name.
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

9. There is a limit to the number of domain names that you can manage using Route 53.

- [ ] A. 10 domain names, but more can be requested
- [ ] B. There is no limit
- [ ] C. 10 domain names, but more can not be requested

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