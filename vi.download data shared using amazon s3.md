# VI. DOWNLOAD DATA SHARED USING AMAZON S3

[LINK TO BACK TO ALL WORKSHOPS](./workshop.md)

# A. AWS CLI COMMANDS FOR S3
Now that you have access to the AWS CLI, you can use it to download files from an S3 bucket.

1. Run an AWS CLI command to list all the S3 buckets associated with the account.
```
aws s3 ls
```
The output will list all the S3 buckets (if any) associated with the account.

For the purpose of this workshop we will be using a named profile with AWS CLI which is a collection of settings and credentials that you can apply to a AWS CLI command. When you specify a profile to run a command, the specific settings and credentials associated with the profile are used to run that command. This is typically followed when credentials are shared to access resources. Use the AWS CLI with the credentials (for the named profile) provided during the training to access the S3 bucket folders shared with us for this workshop.

2. Enter the AWS CLI command to configure AWS Credentials with a profile named “training”.
```
aws configure  --profile training
```
3. Enter the aws access key id provided for the training.
```
AWS Access Key ID [None]: [Access Key ID]
```
4. Next Enter the aws secret key provided for the training.
```
AWS Secret Access Key [None]: [Secret Access Key]
```
5. Enter a Default region name.
```
Default region name [None]: ap-southeast-1
```
Hit enter to accept the defaults for output format

6. Rerun the AWS CLI S3 command to list the folders within an S3 bucket, but this time with the profile. This will show you all folders of the bucket that share the account credentials.
```
aws s3 ls --profile training s3://slchen-lab-transfer/GIS-training/
```
The output is a list of folders within the S3 bucket (slchen-lab-tranfer) that is shared.

Expected output:
```
PRE SRR6327875/
PRE SRR6327950/
```

# B. DOWNLOAD DATA FROM S3 BUCKET
Now that you have access to the shared S3 bucket, we will download data from the S3 bucket to your machine.

1. First, list the folders in the S3 bucket:
```
aws s3 ls --profile training --region ap-southeast-1 s3://slchen-lab-transfer/GIS-training/
```

You will find the sub-folders of the slchen-lab-transfer in the list under the “GIS-training” folder.

> NOTE: Only a specific few folders may have been shared. Users are able to select which folders are available to the public within a bucket.

We will now copy files from a specific location within this folder to your machine.

Before that, let’s create a directory on your machine to store this data.

2. Run the following commands to create a directory to hold the data and cd to that directory:
```
mkdir -p /tmp/fastq/SRR6327950
```
```
cd /tmp/fastq/SRR6327950
```
3. Run the AWS CLI command to copy files from the S3 bucket to this new directory.
```
aws s3 cp --profile training s3://slchen-lab-transfer/GIS-training/SRR6327950/SRR6327950_1.fastq.gz . --region ap-southeast-1
```
```
aws s3 cp --profile training s3://slchen-lab-transfer/GIS-training/SRR6327950/SRR6327950_2.fastq.gz . --region ap-southeast-1
```
4. Verify the MD5 checksum for files copied from S3 as you would normally do.
```
md5sum *.fastq.gz
```
The original MD5 hashes for these files are:

- SRR6327950_1.fastq.gz - 27920396d2eb46f463f93eb9a2baf4e5
- SRR6327950_2.fastq.gz - b46c7cf0d45cf3325222506d41fa7329

5. Download another data set using from the NCBI SRA repository on S3 (in the AWS Repository of Open Data):
```
mkdir -p /tmp/fastq/SRR6327875
cd /tmp/fastq/SRR6327875

# note here the --no-sign-request makes an anonymous request to this public S3 bucket
aws s3 sync --no-sign-request s3://sra-pub-run-odp/sra/SRR6327875/ /tmp/fastq/SRR6327875/

# convert the sra formatted file to fastq, then gzip them and clean up
fasterq-dump ./SRR6327875
gzip SRR6327875_1.fastq
gzip SRR6327875_2.fastq
rm -f SRR6327875
```
We are now ready to run some data analyses.

[LINK TO BACK TO ALL WORKSHOPS](./workshop.md)