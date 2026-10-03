# AWS S3 - Storage

Amazon Simple Storage Service (S3) is an object storage service designed to store and retrieve data at scale. Data is stored as objects in buckets and accessed through APIs, the console, or tools such as the AWS CLI.

## Core concepts

### Buckets

A bucket is a container for objects. Bucket names are globally unique within the AWS partition and are created in a Region. A bucket can hold objects with different prefixes, but S3 has no traditional directory hierarchy; folders in the console are key-name prefixes.

Choose a naming convention that identifies the workload, environment, and data classification. Block public access at the account and bucket level unless a deliberate public use case requires otherwise.

### Objects

An object consists of data, a key, and metadata. The key is the complete name used to retrieve the object, such as `reports/2026/summary.csv`. Objects may also have tags, a storage class, encryption settings, and (when versioning is enabled) a version ID.

S3 is strongly consistent for object reads and listings after successful writes. Use multipart uploads for large files and configure cleanup for incomplete multipart uploads.

### Storage classes

Choose a class based on access frequency, retrieval latency, resilience, and retention:

| Storage class | Typical use |
| --- | --- |
| S3 Standard | Frequently accessed data |
| S3 Intelligent-Tiering | Unknown or changing access patterns |
| S3 Standard-IA | Infrequently accessed data that needs rapid retrieval |
| S3 One Zone-IA | Re-creatable infrequently accessed data in one Availability Zone |
| S3 Glacier Instant Retrieval | Archive data needing millisecond access |
| S3 Glacier Flexible Retrieval | Archive data with minutes-to-hours retrieval |
| S3 Glacier Deep Archive | Lowest-cost long-term archives with long retrieval times |

Minimum storage durations, retrieval fees, request charges, and early deletion charges differ by class. Evaluate total cost rather than storage price alone.

### Versioning

Versioning keeps multiple versions of an object under the same key. It protects against accidental overwrites and deletes, but it also increases storage use. A delete normally creates a delete marker rather than immediately removing older versions.

Enable versioning before storing important data, configure lifecycle rules for noncurrent versions, and use MFA Delete only where its operational trade-offs are acceptable.

### Lifecycle policies

Lifecycle rules automatically transition objects between storage classes or expire objects and versions. Rules can target prefixes or object tags. Common policies move logs to an archive class after a retention period, expire temporary files, and remove noncurrent versions.

Validate lifecycle timing and retention requirements before applying expiration. Lifecycle transitions are asynchronous and may incur request or transition charges.

### Encryption

S3 encrypts new objects by default with server-side encryption. Common choices are:

- **SSE-S3**: S3-managed keys.
- **SSE-KMS**: AWS KMS keys, with key policies, grants, and audit events.
- **DSSE-KMS**: dual-layer server-side encryption for applicable high-assurance requirements.
- **Client-side encryption**: the application encrypts data before sending it to S3.

Use KMS when independent key control, key rotation, or detailed key-use auditing is required. Restrict key permissions as carefully as S3 permissions.

### Bucket policies

A bucket policy is a resource-based JSON policy that controls access to a bucket and its objects. It can grant access to other accounts or AWS services and can require conditions such as TLS (`aws:SecureTransport`), a VPC endpoint, a principal organization, or a specific KMS key.

Keep Block Public Access enabled, require encryption where appropriate, deny insecure transport, and grant only the required prefixes and actions. Combine bucket policies with IAM identity policies, access points, and VPC endpoint policies where relevant. Never rely on a bucket policy alone to protect sensitive data.

## Common use cases

- Backing up files, databases, and infrastructure artifacts.
- Hosting static website assets or application uploads.
- Storing logs, audit records, data lakes, and analytics data.
- Distributing software packages and release artifacts.
- Archiving compliance records with retention and legal-hold controls.

## Recommended baseline

Enable versioning, default encryption, Block Public Access, access logging or CloudTrail data events where needed, lifecycle management, least-privilege policies, and cross-Region replication for disaster recovery requirements. Test restore and object-recovery procedures.

## Further reading

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/)
- [S3 storage classes](https://aws.amazon.com/s3/storage-classes/)
- [S3 security best practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)

