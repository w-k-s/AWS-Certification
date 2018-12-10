# 6. Route53

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

## 6.4 Route53

- Route53 is a global service

**Questions**

1. Route53 is Amazon's DNS Service.

- [ ] A. Correct
- [ ] B. Incorrect

2. Route53 does not support zone apex records (or naked domain names)

- [ ] A. Correct
- [ ] B. Incorrect
- [ ] C. Depends on the circumstances
- [ ] D. Only on US-East 1

3. Which of the following is a valid A name entry 

- [ ] A. www.google.com
- [ ] B. timothy1.s3-website.eu-west-1.awsamazon.com
- [ ] C. 5.30.210.186
- [ ] D. api.facebook.com

4. Which of the following is a valid C name entry 

- [ ] A. www.google.com
- [ ] B. timothy1.s3-website.eu-west-1.awsamazon.com
- [ ] C. 5.30.210.186
- [ ] D. api.facebook.com

4. Which of the following is a valid Alias name entry 

- [ ] A. www.google.com
- [ ] B. timothy1.s3-website.eu-west-1.awsamazon.com
- [ ] C. 5.30.210.186
- [ ] D. api.facebook.com

5. Explain the difference between C names and alias names.

Answers
1. A. Duh! 
2. B. Alias was introduced by AWS to add support for naked domain names.
3. C. `A` names must map to an ip address
4. B. 
5. C names do not support mapping naked domains e.g. `google.com`, whereas alias names do.