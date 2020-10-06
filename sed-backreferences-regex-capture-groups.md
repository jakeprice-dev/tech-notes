---
id: 20201006111453
uuid: 9186397d-093f-4acd-89ed-b73053d11935
title: Sed Backreferences and RegEx Capturing Groups
date: 2020-10-06 11:14:53
modified: 
types: tech-note
categories: sed
pinned: false
tags: [sed, backreferences, regex, capturing-groups]
private: false
draft: false
---

Sed is very powerful, and I've just learnt about how you can utilise regular expression capturing groups, and sed backreferences to do some particuarly powerful edits.

At work we had some synthetic data and I needed to strip the first 3 characters from strings in two specific columns in a 1GB+ CSV. I had to make sure that it was only these columns, so I couldn't just match the first 3 digits, as that would match on all numerical strings. Thankfully the structure of the strings in the `SigningNum` and `SigningNumAdd` columns were unique.

## Command

The `sed` command would look like this:

```sh
sed --regexp-extended --in-place=.orig "s/(,[0-9]{3})([0-9]{2}\ \-)/,\2/g" <file>
```

## Source Data

```csv
Column1,Column2,Column3,Column4,Column5,Column6,Column7,Column8,Column9,Column10,Column11,Column12,Column13,Column14
1GYS4FEJ3CR110987,BD9B6E186B5C0D967911B96F697E697A6DC61D4B,1FTNF2B51AE007310,216.239.174.105,92.151.147.165,Z,10001 - 12/08/1990,2009,FALSE,TRUE,10001 - 12/08/1990,2008,2020-11-30,2020-10-01
JM3TB2BVXD0238933,EECA35C73E66A5A58BA4E763C26B2C4083C684BA,SCBLC43F26C295723,244.218.199.98,64.49.209.152,Z,10002 - 12/08/1990,1986,FALSE,TRUE,10002 - 12/08/1990,2008,2020-11-30,2020-10-01
JN8AZ2NC2E9366591,C607477F79B9CCE088EE6EAC12CB50D9FE70C11C,WBALM5C51AE038487,181.252.242.234,135.155.158.99,Z,10003 - 12/08/1990,1995,FALSE,TRUE,10003 - 12/08/1990,2005,2020-11-30,2020-10-01
```

## Process

The string structure for columns `SigningNum` and `SigningNumAdd` is the same, it's structured like this `NNNNN - DD/MM/YYYY` but we can't specify a column like we can in Excel, so we have to match the string based on something in the structure that is unique to these columns.

It's enough to match based on this pattern `,NNNNN -` where `N` is a number. This is because no other columns have strings like this. We also include the comma delimiter `,` at the start because it helps ensure we're matching the start of the column, and not the middle of a string (if there were other similar patterns to this column).

We then split the pattern into two Regular Expression Capturing Group's.
	
1. Pattern: `,NNN` - Regex to match pattern: `(,[0-9]{3})`
1. Pattern: `NN -` - Regex to match pattern: `([0-9]{2}\ \-)`

Capturing Group 1 then becomes the part of the string we want to remove, and group 2 is the part of the string we want to keep. 
 
It's then just a matter of telling `sed` to do just that in the "replace" section of the command with a backreference to the 2nd capturing group. So `,\2` tells sed to replace `(,[0-9]{3})([0-9]{2}\ \-)` with the contents of the 2nd capturing group, prepended with a comma (so we make sure we still have the delimiter).
 
So you end up with the strings in both columns looking like this: `,NN - DD/MM/YYYY` (where the `N` and `DD/MM/YYYY` will be a number and date) which are stripped of the match in the 1st capturing group, and thus have had the first 3 characters removed.

## Result

The resulting data then looks like the below.
 
```csv
Column1,Column2,Column3,Column4,Column5,Column6,Column7,Column8,Column9,Column10,Column11,Column12,Column13,Column14
1GYS4FEJ3CR110987,BD9B6E186B5C0D967911B96F697E697A6DC61D4B,1FTNF2B51AE007310,216.239.174.105,92.151.147.165,Z,01 - 12/08/1990,2009,FALSE,TRUE,01 - 12/08/1990,2008,2020-11-30,2020-10-01
JM3TB2BVXD0238933,EECA35C73E66A5A58BA4E763C26B2C4083C684BA,SCBLC43F26C295723,244.218.199.98,64.49.209.152,Z,02 - 12/08/1990,1986,FALSE,TRUE,02 - 12/08/1990,2008,2020-11-30,2020-10-01
JN8AZ2NC2E9366591,C607477F79B9CCE088EE6EAC12CB50D9FE70C11C,WBALM5C51AE038487,181.252.242.234,135.155.158.99,Z,03 - 12/08/1990,1995,FALSE,TRUE,03 - 12/08/1990,2005,2020-11-30,2020-10-01
```
