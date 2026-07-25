## - Storage Classes
## s3 - simple storage service (global service)
=======================
```bash
object storage  | block storage 
 
highly scalable service for storing and retrieving any amount of data from anywhere on the web.


EBS - elastic block store --> block storage --> volumes(8gb-16tb) attached to EC2 instances
  |--> server hosting service


S3 - simple storage service --> object storage --> unlimited storage for any type of data (object limit 5tb each)
  |--> serverless hosting service

s3 - bucket --> folder --> object(file)---> unlimited data
bucket - 100 bucket limit per account (by default)--> (region specific)
 |--> bucket name should be globally unique
 |--> bucket name should be in lowercase

object - file + metadata

bucket versioning 
--> keep multiple versions of an object in the same bucket
 have old file version null
 new file have version id uniqid 

acl - access control list
--> to manage access to buckets and objects 
policies - to manage access to buckets and objects

static website hosting
--> host static website using s3 bucket
enable static website hosting in bucket properties
  |--> upload index.html and error.html files
  |--> acl enable 
  |--> set bucket policy to make objects public --> make public using acl
  |--> off block public access
  |--> static website hosting enabled 
  |--> access website using bucket endpoint URL

## storage classes 


server acess logging
--> track requests to access s3 bucket
--> log file delivered to target bucket

object lock 
--> prevent objects from being deleted or overwritten 

costing 
standard storage class
bucket data + data transfer costs ==> to owner of the data

requester pays
bucket data --> bucket owner
data transfer--> requester
but this can not be used in public buckets (static website hosting-buckets)
--> requester pay for the data transfer and request costs
```
## storage classes (https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
```bash
1. frequent access
standard (performance, high availability, high durability) 2.00$
reduced redundancy storage (lesser durability, lesser cost)(not recommended)1.00$

-----intelligent tiering (cost optimization)-------
    --> automatically moves data between two access tiers when access patterns change
    30--> standard access tier
    60--> infrequent access tier
    150--> deep archive tier

2. infrequent access
 standard-IA (less cost, less availability, high durability)1.25$
 one zone-IA (less cost, less availability, lesser durability)1.00$

3. archive (zip or archive type storage)
instantaeous retrieval (1hrs)0.50$
flexible Glacier retrieval(6hrs)0.35$
deep archive (12hrs)0.25$

lifecycle policies
management-->create lifecycle rule
video file life cycle 
Q1. what is lifecycle policy in s3 ?

replication rule 
to get the backup of one s3 bucket in another s3 bucket in different region
replication rule --> same region --> cross region --> creoss account replication

## policy types
bucket policy 
resource based policy --> attached to resource (s3 bucket)
--------------------------------------------------------------------
IAM policy
identity based policy --> attached to user/group/role

bucket policy 

```
- bucket policy - https://awspolicygen.s3.amazonaws.com/policygen.html
- https://aws.amazon.com/s3/storage-classes/intelligent-tiering/




done
