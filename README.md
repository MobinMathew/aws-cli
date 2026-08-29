# aws-cli
first we have to create a account in https://aws.amazon.com/free/?trk=78c55dff-53b9-4938-8ed3-d071891360dd&sc_channel=ps&trk=78c55dff-53b9-4938-8ed3-d071891360dd&sc_channel=ps&ef_id=CjwKCAjw48TUBhBREiwAK0GnQVNSYr2Z1cWwWwH06nKgkH41PN80WC5UVihAqB2fg9A7SJh53bJFLBoCqtoQAvD_BwE:G:s&s_kwcid=AL!4422!3!808712755158!e!!g!!aws!23846236475!198027716802&gad_campaignid=23846236475&gbraid=0AAAAADjHtp-DIRiLRq6ulcxI0ev47JgYd&gclid=CjwKCAjw48TUBhBREiwAK0GnQVNSYr2Z1cWwWwH06nKgkH41PN80WC5UVihAqB2fg9A7SJh53bJFLBoCqtoQAvD_BwE
After creating the account login into your account(root user)
 
# s3--->  Simple Storage Service is s an AWS cloud storage service used to store and retrieve files or data such as images, videos, documents, backups, logs, and application data. S3 stores data inside buckets, and each file is called an object with a unique key (name/path). It provides high durability, scalability, access control, versioning, encryption, and lifecycle management. For example, you can create an S3 bucket called mobin-demo-bucket, upload photo.jpg into it, and later download or access that file using AWS CLI, the AWS Console, or an application. 

search for IAM user 
then click on create user to create the user
 the 

##### DAY 3: 
## 1. Enable versioning

First, make sure your bucket exists:

```bash
aws s3 ls
```

Then enable versioning:

```bash
aws s3api put-bucket-versioning --bucket mobin-demo-bucket --versioning-configuration Status=Enabled
```

### Check whether it is enabled

```bash
aws s3api get-bucket-versioning --bucket mobin-demo-bucket
```

Expected:

```json
{
    "Status": "Enabled"
}
```

**Why versioning?**
It keeps previous versions of an object when you overwrite or delete it, which helps with accidental changes/deletions.

---

## 2. Check bucket security settings

### Check public access blocking

```bash
aws s3api get-public-access-block --bucket mobin-demo-bucket
```

Ideally, you want:

```json
{
    "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "IgnorePublicAcls": true,
        "BlockPublicPolicy": true,
        "RestrictPublicBuckets": true
    }
}
```

This means **public access is blocked**.

### Check bucket ACL

```bash
aws s3api get-bucket-acl --bucket mobin-demo-bucket
```

### Check bucket policy

```bash
aws s3api get-bucket-policy --bucket mobin-demo-bucket
```

If there is no bucket policy, AWS may return a `NoSuchBucketPolicy` error — that's okay; it simply means **no bucket policy is configured**.

### Check encryption

```bash
aws s3api get-bucket-encryption --bucket mobin-demo-bucket
```

If encryption is configured, you'll see the encryption rules.

---

### Your practical checklist

```text
S3 Security
│
├── Versioning              → put-bucket-versioning
├── Public access block     → get-public-access-block
├── ACL                      → get-bucket-acl
├── Bucket policy            → get-bucket-policy
└── Encryption               → get-bucket-encryption
```


