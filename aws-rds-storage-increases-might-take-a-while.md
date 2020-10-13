---
id: 20201013124524
uuid: 1eb9ea2e-e4d1-4b0c-abe3-c3ce66d4bda5
title: RDS Storage Increases Might Take a While
date: 2020-10-13 12:45:24
modified: 
types: tech-note
categories: aws
pinned: false
tags: [rds, terraform, aws, storage, capacity, disk, drive]
private: false
draft: false
---

An increase to the storage of a big RDS instance at work took 38 minutes to complete via Terraform. It was an increase from 300 GiB to 500 GiB.

This is something to bare in mind. Apparently there's always potential for RDS database storage increases to take a good while, as detailed here: [Troubleshoot an Amazon RDS DB Instance That Is Stuck in the Modifying State When Trying to Increase the Allocated Storage](https://aws.amazon.com/premiumsupport/knowledge-center/rds-stuck-modifying/)
