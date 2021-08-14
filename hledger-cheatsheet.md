---
id: 20210814220033
uuid: 18cab5f8-6ec5-4fbd-9e6b-9ed3f1ae240c
title: hledger Cheatsheet
date: 2021-08-14 22:00:33
modified: 
types: tech-note
categories: hledger
pinned: false
tags: [hledger, ledger, accounting, finance, money]
private: false
draft: false
---

{{<toc>}}

## Equity

> The most confusing transaction in any ledger will be your equity account—because starting balances can’t come out of nowhere.
>
> When you first start your ledger, you will likely already have money in some of your accounts. Let’s say there’s $100 in your checking account; then add a transaction to your ledger to reflect this amount. Where will the money come from? The answer: your equity.
>
> ```ledger
> 10/2  Opening Balance
>    	Assets:Checking                         $100.00
>   	Equity:Opening Balances
> ```
> But what is equity? You may have heard of equity when people talked about house mortgages, as “the part of the house that you own”. Basically, equity is like the value of something. If you own a car worth $5000, then you have $5000 in equity in that car. In order to turn that car (a commodity) into a cash flow, or a credit to your bank account, you will have to debit the equity by selling it.
>
> When you start a ledger, you probably already have a net worth. Your net worth is your current equity. By transferring the money in the ledger from your equity to your bank accounts, you are crediting the ledger account based on your prior equity. That is why, when you look at the balance report, you will see a large negative number for Equity that never changes: Because that is what you were worth (what you debited from yourself in order to start the ledger) before the money started moving around. If the total positive value of your assets is greater than the absolute value of your starting equity, it means you are making money.
>
> Clear as mud? Keep thinking about it. Until you figure it out, put `not Equity` at the end of your balance command, to remove the confusing figure from the total.
> -- https://www.ledger-cli.org/3.0/doc/ledger3.html

## Commands

```sh
# Quite helpful when adding transactions and checking things balance as you go:
hledger-web --file <journal-file>

# Get all transactions matching a description:
hledger register --file <journal-file> "desc:hospital"

# Get the balance of all transactions matching a description or tag with a breakdown by account:
hledger balance expenses -f <journal-file> "desc:amazon"
hledger balance expenses -f <journal-file> "tag:camera-install"

# Get the balance of all savings accounts:
hledger balance --file <journal-file> "acct:savings"

# Get the balance of a tag:
hledger balance --file <journal-file> "tag:camera-install"

# Get the balance of all savings accounts between a period:
hledger balance --file <journal-file> --period "2022-01-01 to 2022-01-18" "acct:savings"

# Print transactions (journal entries) matching description:
hledger print --file <journal-file> "desc:TODO"

# Print statement across all accounts for period and output as HTML:
hledger incomestatement --file <journal-file>  --period "2022-01-01 to 2022-01-31" --output-file income_statement.html
hledger incomestatement --file <journal-file>  --period "jan" --output-file income_statement.html
hledger incomestatement --file <journal-file>  --period "2022-02" --output-file income_statement.html

# Balance sheet and exclude accounts:
hledger balancesheet -f 2022.journal "not:liabilities:debts:credit_account:example" "not:liabilities:debts:loans:example"
```

