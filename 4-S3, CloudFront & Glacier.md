# 4. AWS Object Storage and CDN - S3, Glacier and CloudFront

## 4.1 - S3

### 4.1.1 Basics

- Object based storage (upload files only. Can't upload Operating systems or Databases)
- File size: 0 - 5 TB
- Unlimited Storage but paid
- Files are stored in buckets (a bucket is a folder that has a unique namespace)
- S3 is a universal namespace: all names must be universally unique
- [!] URL Structure of an S3 bucket is as follows

`https://s3-[region-name].amazonaws.com/[bucketname]`

- When a file is uploaded successfully, a HTTP 200 response is returned.

### 4.1.2 Data Consistency Model

- **Read after Write consistency for PUTS of new objects**

This means that once the file has successfully been written to the S3 bucket, a user will be able to **immediately** read that file (immediate consistencty)

- **Eventual Consistency for overwrite PUTS and DELETES (can take some time to propogate)**

This means that if an existing file is updated or deleted, it may take some time for the changes to propogate across availability zones. In otherwords, rereading the file immediately after making changes may show the old version or the up-to-date version.

### 4.1.3 Key-Value Storage

- Object based
- Object consits of:
- **Key**: the name of the file
- **Value**: A bag of bytes
- **Version ID**: Version of the file (`null`, unless Versioning is enabled)
- **Metadata**: data about the file
- **Subresources** e.g. Access Control Lists (list of users, groups roles that can access the file)

### 4.1.4 Storage Classes

**S3 Standard**

- **99.99% availability**: Files will always be available to access
- **99.9999999999% durability**: Files are guaranteed to not be lost.
- Stored redundantly on multiple devices in multiple availability zones.
- Designed to sustain loss of 2 availability zones

**S3 - IA (Infrequenct Access)**

- files that are accessed less frequently, but need rapid access
- Cheaper than S3 standard.
- Retrieval fee per GB retrieved.

**S3 One Zone - IA (Reduced Redundancy Storage, RRS)**

- Same as S3-IA, however file is stored in only one availability zone, instead of two
- hence cheaper than S3-IA.
- used for storing data whose loss can be tolerated.

**Glacier**

- Files that aren't access frequently at all (i.e. archived) and do not require rapid access.
- Expedited: Retrieval will take a few minutes 
- Standard: Retrieval takes 3 - 5 hours
- Bulk: Retrieval takes 5 - 12 hour

### 4.1.5 Charges

Charged for:

- Storage (per GB stored)
- Requests (Requests per minute)
- Storage Managemnent Pricing (charged for tags and metadata attached to the Object)
- Data Transfer Pricing (When transferring data from one region to another: Cross-Region Replication)
- S3 Transfer Acceleration (users can upload file to Cloudfront and the file will be transferred to S3 from the edge location rather than directly from the user's network. This accelerates the rate of transfer of the file.)

> **Exam Tip**: Read S3 FAQ before the exam as it is a prominently featured topic.

### 4.1.6 Access Control

- Buckets are private and all objects stored inside them are private.
- If an object is public, but then it is updated, the object's access will be reset to private.
- Access to an object is controlled by Bucket Policies and by ACL (Access Control Lists)

### 4.1.7 Encryption

There are two kinds of Encryption

1. In Transit: when data is being sent to and from the bucket. This is secured using HTTPS.
2. At Rest: Encryption that occurs when the file is stored on the client or on the server.

- Encrypted with Amazon S3 managed keys (SSE-S3) using AES-256 keys
- Encrypted with KMS keys (SSE-KMS) (Key Management Service) 
- Encrypted with customer provided keys (SSE-C)

The KMS key keeps track of all uses of the key. Access permissions can also be set on the KMS envelope key (guarding who can use the key). In other words, KMS has extra security compared to SSE-S3

### 4.1.8 Versioning

- Stores all version of the document (including all writes and even when an object is deleted)
- User pays for the storage of each version.
- Once versioning is enabled, it can only suspended. It can not be disabled.
- When a file is deleted, a new version is created with a delete marker.
- Multi Factor Authentication (MFA) rules can be applied to deleting an object in a bucket.

### 4.1.9 Cross-Region Replication

- Cross-region replication is the copying of objects from one bucket in one region to a bucket in another region
- Source and destination buckets must have versioning enabled.
- Once Cross-Region Replication is configured, files are not replicated automatically. Updates to the files will be replicated.
- Delete markers are not replicated.
- Deleting a version of an object is not replicated.

### 4.1.10 Lifecycle Management

- Automtically transitions objects (and their versioned) to tiered storage e.g. Transition object to S3 - IA after 30 days and transition object to Glacier 30 days it was transitioned to s3-ia.
- Lifecycle management can be applied to both object and its versions.
- Allows you permanently delete files if not access.
- Useful to cut down on costs storing files that are not used accesed very often.

### 4.1.11 Static Websites

- URL Structure: `http://<bucket-name>.s3-website.<region-name>.amazonaws.com`

## 4.2 CloudFront

- Edge location is a location where data will be cached.

- Origin is the origin of all the files that the CDN will distribute (i.e. the location that hosts the original content). The origin is usually an EC2 instance, an S3 bucket or an Elastic load balancer

- Distribution: The name given to the CDN which consists of a collection of edge locations. 
There are two kinds of distributions: Web (for website caching) and RMTP (which is used for streaming flash files)

- Objects are stored in cloud front for as long as their TTL (Time-To-Live, measured in seconds).

- The cache can be cleared but this will incur a charge.


## 4.4 Storage Gateway

- AWS Storage Gateway is a hybrid storage service that enables your on-premises applications to seamlessly use AWS cloud storage
-  An applications connect to the service through a virtual machine or hardware gateway appliance using standard storage protocols, such as NFS, SMB and iSCSI. The gateway connects to AWS storage services, such as Amazon S3, Amazon Glacier, and Amazon EBS, providing storage for files, volumes, and virtual tapes in AWS
- The service can be used for backup and archiving, disaster recovery, cloud data processing, storage tiering, and migration
- The service includes a highly-optimized data transfer mechanism, with bandwidth management, automated network resilience, and efficient data transfer, along with a local cache for low-latency on-premises access to your most active data.

### 4.4.1 Types of Storage Gateways

- **File Gateway (NFS)**
- **Volume Gateways (iSCSI)**
- **Tape Gateway (VTL)**

#### 4.4.1.1 File Gateway

- Files are stored by the gateway to an S3 bucket.
- Files must be flat e.g. txt, pdf, images
- Files are acccessed using the NFS protocol
- Can be managed as native S3 objects

![alt text][logo]

[logo]: https://github.com/w-k-s/AWS-Certification/blob/master/images/filegatewaydiagram.png) "File Gateway Diagram"

#### 4.4.1.2 Volume Gateway(iSCSI)

- Full disk volumes are backed-up by the gateway as EBS Snapshots on S3.
- Snapshots are incremental backups that capture only changed blocks of memory. Snapshots are compressed to conserve space.
- Files are acccessed using the iSCSI (eye-scuzzy) protocol.

Two Types:


- **Stored**: Entire data set is stored onsite and is asynchronously backed up to S3

![alt text][logo]

[logo]: https://github.com/w-k-s/AWS-Certification/blob/master/images/volumegatewaydiagram-stored.png) "Volume Gateway Diagram (Stored)"

- **Cached**: Entire data set is stored on S3 and the most frequently access data is stored on site.


![alt text][logo]

[logo]: https://github.com/w-k-s/AWS-Certification/blob/master/images/volumegatewaydiagram-cached.png) "Volume Gateway Diagram (Cached)"

#### 4.4.1.3 Tape Gateway(VTL)

- Used for backup and uses popular backup applications like NetBackup, BackUp Exec and Veeam.
- Data is backed up into virtual tapes which are then stored in S3
- Add tape cartridges to S3 as needed.

## 4.5 Snowball

Snowball is based of an old Amazon service called **Import/Export** where customers would send in their hard-disks and amazon would upload the data to S3. Different device types became difficult to manage to Amazon introduced standard storage devices known as snowballs.

There are three kinds of snowballs

1. Snowball
2. Snowball Edge
3. Snowmobile

Snowballs are reused by Amazon however the disk is cleared out securely by amazon to ensure other customers can not access the data by previous customers.

### 4.5.1 Snowball

- Snowball is an appliance capable storing 80TB of data. Customers upload data on to the appliance, send it back to Amazon where the data is exported to S3.

### 4.5.2 Snowball edge

- Like Snowball, Snowball edge is an appliance that is used to store data however, this appliance has compute capability; meaning, for example, that it can be used to host lambdas.
- **Application Example**: A snowball edge can be boarded on a plane where it can be used to not only store but also process fligth data. After the flight, the Snowball edge can be removed from the aircraft and the data can be uploaded to S3.

### 4.5.3 Snowmobile

- A massive shipping container capable of storing 100 petabyte. 

## 4.6 S3 Traffic Acceleration

- AWS Regions are only located in certain regions of the world and uploading to an S3 bucket to an availability zone in that region might take a while.
- S3 Traffic Acceleration uploads the file to a CloudFront edge location near the user. Once uploaded, the file is uploaded to the S3 bucket using AWS' backbone network.

- S3 Traffic acceleration URL looks like: <bucket-name>.s3-accelerate.amazonaws.com

## Quiz:

1. S3 has what consistency model for PUTS of new objects

- Read After Write Consistency
- Write AFter Read Consistency
- Eventual Consistency
- Usual Consistency

2. What is AWS Storage Gateway?

- It's an on-premise virtual appliance that can be used to cache S3 locally at a customers site.
- It allows large scale import/exports in to the AWS cloud without the use of an internet connection.
- It allows a direct MPLS connection in to AWS.
- None of the above.

3. One of your users is trying to upload a 7.5GB file to S3 however they keep getting the following error message - "Your proposed upload exceeds the maximum allowed object size.". What is a possible solution for this?

- Design your application to use the multi-part upload API for all objects
- Design your application to use large object upload API for this object
- Raise a ticket with AWS to increase your maximum object size
- Log in to the S3 console, click on the bucket and then click properties. You can then increase your maximum object size to 1TB

4. What does RRS stand for when talking about S3?

- Relational Reaction Storage 
- Reduced Redundancy Storage
- Regional Rights Storage
- Redundancy Reduced Systems

5. You have been asked by your company to create an S3 bucket with the name "timothy123" in the EU West region. What would be the URL for this bucket?

- https://s3-us-east-1.amazonaws.com/timothy123
- https://s3.timothy123.amazonaws.com/eu-west-1
- https://s3-timothy123.amazonaws.com/
- https://s3-eu-west-1.amazonaws.com/timothy123

6. What is Amazon Glacier?

- A tool used to freeze an EBS volume
- An AWS Service designed for long term archival
- A tool used to ressurrect deleted EC2 snapshots
- A highly secure firewall.

7. What does S3 stand for?

- Straight Storage Service
- Simplified Serial Sequency
- Simple Storage Service
- Simple SQL Service

8. You are a solutions architect who works with a large digital media company. The company has decided that they want to operate within the Japanese region and they need a bucket called "testbucket" set up immediately to test their web application on. You log in to the AWS console and try to create this bucket in the Japanese region however you are told that the bucket name is already taken. What should you do to resolve this?

- Change your region to Korea and then create the bucket "testbucket". 
- Raise a ticket with AWS and ask them to release the name "testbucket" to you.
- Bucketnames are global, not regional. This is a popular bucket name and is already taken. You should choose another bucket name.
- Run a WHO IS request on the bucket name and get the registered owners email address. Contact the owner and ask if you can purchase the rights to the bucket. 

9. What is the availability on RRS?

- 99.9%
- 99%
- 99.99%
- 99.99999999999%

10. What is the durability on RRS?

- 99.9%
- 99%
- 99.99%
- 99.99999999999%

11. What is the durability on S3?

- 99.9%
- 99%
- 99.99%
- 99.99999999999%

12. What is the availability on S3?

- 99.9%
- 99%
- 99.99%
- 99.99999999999%

13. What is the minimum file size that I can store on S3?

- 1 KB
- 1 MB
- 1 GB
- 1 byte

14. The difference between S3 and EBS is that EBS is object based where as S3 is block based.

- True
- False

15. S3 has eventual consistency for which HTTP Methods?

- PUTS of new Objects and DELETEs
- overwrite PUTs and DELETEs
- PUTs of new objects and UPDATEs
- UPDATEs and DELETEs

16. You work for a busy digital marketing company who currently store their data on premise. They are looking to migrate to AWS S3 and to store their data in buckets. Each bucket will be named after their individual customers, followed by a random series of letters and numbers. Once written to S3 the data is rarely changed, as it has already been sent to the end customer for them to use as they see fit. However on some occassions, customers may need certain files updated quickly, and this may be for work that has been done months or even years ago. You would need to be able to access this data immediately to make changes in that case, but you must also keep your storage costs extremely low. The data is not easily reproducible if lost. Which S3 storage class should you choose to minimise costs and to maximise retrieval times?

- S3
- S3 - IA
- S3 - RRS
- Glacier

17. You need to use an Object based storage solution to store your critical, non replaceable data in a cost effective way. This data will be frequently updated and will need some form of version control enabled on it. Which S3 storage solution should you use?

- S3
- S3 - IA
- S3 - RRS
- Glacier

18. You work for a health insurance company who collects large amounts of documents regarding patients health records. This data will be used usually only once when assessing a customer and will then need to be securely stored for a period of 7 years. In some rare cases you may need to retrieve this data within 24 hours of a claim being lodged. Which storage solution would best suit this scenario? You need to keep your costs as low as possible.

- S3
- S3 - IA
- S3 - RRS
- Glacier

19. You run a meme creation website that frequently generates meme images. The original images are stored in S3 and the meta data about the memes are stored in DynamoDB. You need to store the memes themselves in a low cost storage solution. If an object is lost, you have created a Lambda function that will automatically recreate this meme using the original file in S3 and the metadata in Dynamodb. Which storage solution should you consider to store this non-critical, easily reproducible data on in the most cost effective solution as possible?

- S3
- S3 - IA
- S3 - RRS
- Glacier

20. You run a popular photo sharing website that is based off S3. You generate revenue from your website via paid for adverts, however you have discovered that other websites are linking directly to the images on your site, and not to the HTML pages that serve the content. This means that people are not seeing your adverts and every time a request is made to S3 to serve an image it is costing your business money. How could you resolve this issue?

- Use CloudFront to serve the static content.
- Remove the ability for images to be served publicly to the site and then used signed URL's with expiry dates.
- Use security groups to blacklist the IP addresses of the sites that do this.
- Use EBS rather than S3 to store the content. 

21. A bucket named `timothy1` is created in the `eu-west-a` region. What will it's URL be?

22. A file `blueprint.html` is uploaded to `timothy1`; if the file is uploaded successfully, what HTTP status code can you expect?

23. Once `blueprint.html` is uploaded, how long will it take until the file becomes accessible? What is this type of consistency model called?

24. An update `blueprint.html` is uploaded. How long will it take until the update to the file becomes visible? What is this type of consistency model called?

25. The URL to `blueprint.html` is shared. When it is accessed, an error is displayed. Why is this? and how can it be fixed? 

26. It is desired to convert `blueprint.html` into a website. What will it's website URL be? Can `blueprint.html` be set as the index page? 

27. What is a cloud gateway? 

28. State two differences between `File Gateway` and `Volume Gateway`?

29. What is the difference between `Stored Volume Gateway` and `Cached Volume Gateway`?

30. Your company decides wants to encrypt a fil, `SecretFile.txt`, stored on S3. Options are SSE-S3 and SSE-KMS; what advantage does KMS have over SSE-S3?

31. How can `SecretFile.txt` be secured while it is in transit.

32. Consider an S3 bucket where versioning has not yet been enabled. `file1 ` is uploaded to this bucket. Versioning is then enabled. two files are then uploaded: `file2` and `file3`. An update is made to `file2`.
`file3` is deleted.
Which of the following statement correctly describes the number of versions each file has:

- [ ] A. `file1`: 1 version, `file2`: 2 versions, `file3`: None.
- [ ] B. `file1`: 1 version, `file2`: 2 versions, `file3`: 2 versions.
- [ ] C. `file1`: None, `file2`: 2 versions, `file3`: 2 versions.
- [ ] D. `file1`: None, `file2`: 2 versions, `file3`: 1 version.

33. A file has been stored in Glacier as it is rarely accessed. However, due to an emergency, the file needs to be accessed immediately. What kind of retrievel is needed:

- [ ] A. Expedited
- [ ] B. Standard
- [ ] C. Bulk

33. A file has been stored in Glacier as it is rarely accessed. A file needs to retrieved, but it is not urgent so the cheapest option is used. How many hours will it take to retrieve the file

- [ ] A. A few minutes
- [ ] B. 3 - 5 hours
- [ ] C. 5-12 hours

35. A file is cached on CloudFront. What needs to be set to control when the cached version of the file will be expired. Can the cache of the file be cleared earlier than this?

36. State two requirements for cross-region replication to work

37. After Cross-region replication is enabled, how can the files from one bucket be copied to the other bucket?

38. After Cross-region replication is enabled, how can the updates to one file be copied to the same file in the other region?

39. After Cross-region replication is enabled, will deleting the original file cause the copy in the other region to be deleted?

Answers: 
1. A
2. A
3. A
4. B
5. D
6. B
7. C
8. C
9. C
10. C
11. D
12. C
13. D
14. False
15. B
16. B
17. A
18. D
19. C
20. B
21. `https://s3-eu-west-a.awsamazon.com/timothy1`
22. HTTP 200 OK
23. PUTS of new objects are immediately available (Read after write consistency)
24. update PUTS could happen instantly or take a while (eventual consistency)
25. Buckets and their objects are private by default. To make the link accessible, the bucket must be made public and then the file must be made public.
26. `timothy1.s3-website.eu-west-a.awsamazon.com`. Yes, `blueprint.html` can be set as the index page.
27. AWS Storage Gateway enabled hybrid cloud storage. It connects on-premise applications with S3 storage.
28. `File Gateway` can only store flat files; and it uses the NFS protocol.
`Volume Gateway` can store block files; it uses the ISCSI protocol.
29. `Stored Volume Gateway`: all data is stored locally, asynchronously backed up to S3. `Cache Volume Gateway`: all data is stored on S3. frequently accessed data is cached locally.
30. KMS audits each usage of the key. KMS has special permissions for the envelope key (the key that is used to protect the encryption key)
31. HTTPs can be used for securing files when they are in transit.
32. C. `file1`: None, `file2`: 2 versions, `file3`: 2 versions.
33. A. Expedited retrieval.
34. C. Bulk retrieval takes 5 - 12 hours.
35. The `Time-To-Live (TTL)` needs to be set. The cache can be cleared before then, but it will incur a charge.
36. Source and Destination buckets must be in different regions. Both buckets must have versioning enabled.
37. Using the `s3 cp` from the AWS CLII
38. Updates to files are automatically replicated between the two regions
39. No. Deletes are not replicated across regions.