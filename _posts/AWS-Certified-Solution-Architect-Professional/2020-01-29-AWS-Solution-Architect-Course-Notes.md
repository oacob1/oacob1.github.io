
# AWS Course Notes

<!-- ![My helpful screenshot](/assets/img/avatar.png) -->

## Data Stores

### Data Persistance
- Persistent Dat Store (Data is durable and sticks around after reboots, restarts, or power cycles)
  - Glacier, RDS
- Transient Data Store (Data is just temporarily stored and passed along to another process or persistent store)
  - SQS, SNS
- Ephemeral Data Store (Data is lost when stopped)
  - EC2 Instance Store
  - Memcached

### IOPS vs. Throughput
- Input/Output per Second (IOPS), measure of how fast we can read and write to a device
- Throughput, measure of how much data can be moved at a time

### Consistency Model - ACID & BASE
- ACID
  - Atomic - Transactions are "all or nothing"
  - Consistent - Transactions must be valid
  - Isolated - Transactions can't mess with one another
  - Durable - Completed transaction must stick around
- BASE
  - Basic Availability - values availability even if stale
  - Soft-state - might not be instantly consistent across stores
  - Eventual Consistency - will achieve consistency at some point

### Amazon S3
- **Object Store**
  - `s3://bucket/finance/april/16/invoice_45678.pdf` = a **KEY**, not file path
- Used in other AWS services - directly and behind the scenes
- Maximum object size is 5TB
- Largest object in a single PUT is 5GB
- Recommended to use multi-part uploads if larger than 100MB

#### S3 Consistency

<div class="overflow-table" markdown="block">

| AWS Documentation Statement                                                                   | What S3 is Thinking                                                                                                                                                                                                                                                               |
| :-------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "S3 provides read-after write consistency for PUTs of new objects."                           | Cool, I've never seen this object, and no-one has asked about it before. Welcome aboard, new object - and you can read it immediately                                                                                                                                             |
| "HEAD or GET requests of a key before and object exists will result in eventual consistency." | Wait a second, someone already asked about this key, and i told them, "never saw it". I remember that, and need to honor that response until I completely write this new object and fully replicate it. So, I'll let you read it eventually.                                      |
| "S3 offers eventual consistency for overwrite PUTs and DELETEs."                              | ok, so you want to update or delete an object. Let's make sure we get that update or delete completed locally, then we can replicate it to other places. Until then, I have to serve up the current file. I'll serve up the update/delete once its fully replicated - eventually. |
|"Updates to a single key are atomic." | Whoa, there. Only one person can update this object at a time. If I get two requests, I'll process them in order of their timestamp and you'll see the updates as soon as I replicate elsewhere. |

</div>

### IAM
- By default, any new IAM user you create in an AWS account has no permission
  policies attached
- All permissions polices have an 'Implicity Deny' - no ALLOW = Implicit Deny
- DENY always overrides any ALLOW
- IAM porivides identity services - but also coordinates with STS to allow
  Identity Federation (the user of external identities), to access AWS resources

### **BEST practice** for IAM
- Delete your root access keys
- Activate MFA on your root account
- Create and use an IAM user with Admin privileges instead of the Root Account
- Create individual IAM users
- Use groups to assign permissions
= Follow the "principle of least privilege"
- Apply and IAM password policy
