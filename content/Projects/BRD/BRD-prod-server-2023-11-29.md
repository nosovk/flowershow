# BRD servers
## Current Env
### production
- **disk** - 128Gb (13Gb used) (fast SATA\SAS HDD or slow SATA SSD)
- **cpu** - 8vcpu (Intel(R) Xeon(R) Gold 6134)
- **ram** - 8Gb (3Gb used)
- **net** - 0.5Gbps (Ukraine)
- **OS** - CentOS Stream release 8
- **docker** - 24.0.5
- **uptime** - 116 days
- **envs** - production
- **node type** - worker

### staging
- **disk** - 128Gb (18Gb used) (fast SATA\SAS HDD or slow SATA SSD)
- **cpu** - 10vcpu (Intel(R) Xeon(R) Gold 6134)
- **ram** - 18Gb (8Gb used)
- **net** - 0.5Gbps (Ukraine)
- **OS** - CentOS Stream release 8 
- **docker** - 24.0.5 
- **uptime** - 116 days
- **envs** - feature + staging
- **node type** - manager

[cpu link](https://www.intel.com/content/www/us/en/products/sku/120493/intel-xeon-gold-6134-processor-24-75m-cache-3-20-ghz/specifications.html)

## New Production
In current env, we do not store assets. You may see, that HDD at current servers are not used at all. Also, we got info from you, that there is a plan to store near 1 TB of assets at PIMCore.
We suggest to use S3 storage provider inside PIM to move assets to cloud. This way of working with assets is standard for PIMCore — see [docs](https://pimcore.com/docs/pimcore/10.2/Development_Documentation/Installation_and_Upgrade/System_Setup_and_Hosting/File_Storage_Setup.html). Any S3 compatible storage could be used.
For example:

| Name | Price for 1TB |
|---- | ---- |
| CloudFlare R2 | [\<20 usd](https://developers.cloudflare.com/r2/pricing/) |
| Google Cloud Bucket | [\<25 usd](https://cloud.google.com/storage/pricing#europe) |
| AWS S3 | [\<25 usd](https://aws.amazon.com/ru/s3/pricing/) |
In common all S3 providers are pretty similar in terms of pricing.

> Also, we recommend to back up assets separately, at least once in a month, into separate bucket. It could be costly, but if you remove asset from PIM it's removed from S3 storage also, that could cause data loss. We have incidents like that before, that's why we recommend to back up S3 storage to another S3 bucket.

Current production ENV seems to pretty enough to handle workloads, except storage, that was mentioned above. The weak point of current server is a slow HDD, if you could - please better use fast enterprise SSD at new setup.

Proposed configuration for Prod ENV:
- **disk** - 128Gb (fast SSD)
- **cpu** - 10-12vcpu
- **ram** - 8-12Gb (3Gb used)
- **net** - 1Gbps (Ukraine)