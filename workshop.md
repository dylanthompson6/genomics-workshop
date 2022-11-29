# II. START A VIRTUAL LINUX MACHINE
This workshop section walks you through setting up your own EC2 Linux instance and gain some familiarity with the EC2 service.

This section covers:ß

1. Signing in to the AWS Management Console and exploring it.
2. Creating an Amazon EC2 instance and the related configurations.
3. SSHing into an EC2 instance and run Linux commands.
4. Stopping and Terminating the Linux instance.

[START A VIRTUAL LINUX MACHINE](./II.start%20a%20virtual%20linux%20machine.md)

---

# III. START A LINUX MACHINE FROM A SHARED AMI
This workshop section walks you through setting up your own EC2 Linux instance from a premade AMI, which you will use to run analyses in later sections of the workshop.

We’ll be using an image built by the Chen Lab at NUS and GIS. This has a lot of common tools useful for bacterial (and general) genomics installed. We have documented what is on this AMI and how it was set up on our accompanying GitHub repository. Those instructions may be useful if you want to set up the same software on another system and may help you with some hints when installing other software as well for your own work.

Specifically, you learn how to:

1. Sign in to the AWS Management Console and explore it.
2. Create an Amazon EC2 instance from an AMI.
3. SSH into an EC2 instance and run Linux commands.

[START A LINUX MACHINE FROM A SHARED AMI](./III.start%20a%20linux%20machine%20from%20a%20shared%20AMI.md)

---

# IV. ATTACH ADDITIONAL STORAGE
The created EC2 instance has a root volume with enough storage capacity to support the operating system, as well as a few additional applications required for supporting the analyses. We will now need additional storage to accommodate the large datasets we are about to download (e.g. Raw reads, aligned sequences etc.). Hence we need to attach additional storage.

In this section we will:

1. Create an Elastic Block Store (EBS) volume.
2. Attach the new volume to an EC2 instance.
3. Format and mount the new volume.
    a. Format the new volume with a filesystem.
    b. Mount the volume and copy some data into it.

[ATTACH ADDITIONAL STORAGE](./Iv.attach%20additional%20storage.md)

---

# V. AWS CLI
In this section we will introduce the AWS CLI (Command Line Interface) which can be used to work with AWS resources like EC2 Instances and S3 storage services.

In this section you will:

1. Get introduced to AWS CLI.
2. Verify AWS credentials.

[AWS CLI](./v.aws%20cli.md)

---

# VI. DOWNLOAD DATA SHARED USING AMAZON S3
In this section we will use the AWS CLI (Command Line Interface) to work with AWS resources like EC2 Instances and S3 Storage Services. In this section you will:

1. Learn how to use AWS CLI commands for S3.
2. Download data from Amazon S3 Storage Service.

[DOWNLOAD DATA SHARED USING AMAZON S3](./vi.download%20data%20shared%20using%20amazon%20s3.md)

---

# VII. RUN A BASIC ANALYSIS
In this section we will run bioinformatic analyses on the EC2 instance.

Here you will:
1. (Bacterial Genomics trainings) Run bacterial genome analyses i.e map FASTQ reads to a reference sequence, call SNPs and assemble a genome in Bash.
2. (Long Read RNA-Seq trainings) Run transcript discovery and quantification with long-read RNA-Seq analysis using R.
3. (Single-cell analysis trainings) Examining single cell datasets using R:gene-cell matrices and Seurat objects

[RUN A BASIC ANALYSIS](./vii.run%20a%20basic%20analysis.md)