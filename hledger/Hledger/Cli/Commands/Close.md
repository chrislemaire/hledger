## close

`close [--retain | --migrate | --open] [QUERY]`

By default:
prints a transaction that zeroes out ("closes") all accounts,
transferring their balances to an equity account.
Query arguments can be added to override the accounts selection.
Three other modes are supported:

`--retain`:
prints a transaction closing revenue and expense balances.
This is traditionally done by businesses at the end of each accounting period;
it is less necessary in personal and computer-based accounting,
but it can help balance the accounting equation A=L+E.

`--migrate`:
prints a transaction to close asset, liability and most equity balances,
and another transaction to re-open them.
This can be useful when starting a new file (for performance or data protection).
Adding the closing transaction to the old file allows old and new files to be combined.

`--open`:
as above, but prints just the opening transaction.
This can be useful for starting a new file, leaving the old file unchanged.
Similar to Ledger's equity command.

_FLAGS

You can change the equity account name with `--close-acct ACCT`.
It defaults to `equity:retained earnings` with `--retain`,
or `equity:opening/closing balances` otherwise.

You can change the transaction description(s) 
with `--close-desc 'DESC'` and `--open-desc 'DESC'`.
It defaults to `retain earnings` with `--retain`,
or `closing balances` and `opening balances` otherwise.

Just one posting to the equity account will be used by default,
with an implicit amount.

With `--x/--explicit` the amount will be shown explicitly,
and if it involves multiple commodities, a separate posting
will be generated for each commodity.

With `--interleaved`, each equity posting is shown next to the 
corresponding source/destination posting.

The default closing date is yesterday or the journal's end date, whichever is later.
You can change this by specifying a [report end date](#report-start--end-date);
the last day of the report period will be the closing date.
Eg `-e 2022` means "close on 2022-12-31".

The default closing date is yesterday, or the journal's end date, whichever is later.
You can change this by specifying a [report end date](#report-start--end-date);
(The report start date does not matter.)
The last day of the report period will be the closing date;
eg `-e 2022` means "close on 2022-12-31".
The opening date is always the day after the closing date.

### close and costs

With `--show-costs`, any amount costs are shown, with separate postings for each cost.
(This currently the best way to view investment assets, showing lots and cost bases.)
If you have many currency conversion or investment transactions, it can generate very large journal entries.

### close and balance assertions

Balance assertions will be generated, verifying that the accounts have been reset to zero
(and then restored to their previous balances, if there is an opening transaction).

These provide useful error checking, but you can ignore them temporarily with `-I`,
or remove them if you prefer.

You probably should avoid filtering transactions by status or realness
(`-C`, `-R`, `status:`), or generating postings (`--auto`),
with this command, since the balance assertions would depend on these.

Note custom posting dates spanning the file boundary will disrupt the balance assertions:

```journal
2023-12-30 a purchase made in december, cleared in january
    expenses:food          5
    assets:bank:checking  -5  ; date: 2023-01-02
```

To solve that you can transfer the money to and from a temporary account,
in effect splitting the multi-day transaction into two single-day transactions:

```journal
; in 2022.journal:
2022-12-30 a purchase made in december, cleared in january
    expenses:food          5
    equity:pending        -5

; in 2023.journal:
2023-01-02 last year's transaction cleared
    equity:pending         5 = 0
    assets:bank:checking  -5
```

### Example: retain earnings

<!-- XXX update -->

Record 2022's revenues/expenses as retained earnings on 2022-12-31,
appending the generated transaction to the journal:
 
```shell
$ hledger close --retain -f 2022.journal -p 2022 >> 2022.journal
```

Now 2022's income statement will show only zeroes.
To see it again, exclude the retain transaction. Eg:
```shell
$ hledger -f 2022.journal is not:desc:'retain earnings'
```

### Example: migrate balances to a new file

Close assets/liabilities/equity on 2022-12-31 and re-open them on 2023-01-01:

```shell
$ hledger close --migrate -f 2022.journal -p 2022
# copy/paste the closing transaction to the end of 2022.journal
# copy/paste the opening transaction to the start of 2023.journal
```

<!--
Or, you can automate more by generating one transaction at a time:

```shell
$ hledger close --close -f 2022.journal -p 2022 >> 2023.journal  # do this one first
$ hledger close --open  -f 2022.journal -p 2022 >> 2022.journal
```
-->

Now 2022's balance sheet will show only zeroes, indicating a balanced accounting equation.
([Unless](/investments.html#a-more-correct-entry) you are using @/@@ notation - in that case, try adding --infer-equity.)
To see it again, exclude the closing transaction. Eg:
```shell
$ hledger -f 2022.journal bs not:desc:'closing balances'
```

### Example: excluding closing/opening transactions

When combining many files for multi-year reports, 
the closing/opening transactions cause some noise in reports like `print` and `register`.
You can exclude them as shown above, but `not:desc:...` could be fragile,
and also you will need to avoid excluding the very first opening transaction,
which can be awkward. Here is a way to do it, using tags:
add `clopen:` tags to all opening/closing balances transactions except the first,
like this:

```journal
; 2021.journal
2021-06-01 first opening balances
...
2021-12-31 closing balances  ; clopen:2022
...
```

```journal
; 2022.journal
2022-01-01 opening balances  ; clopen:2022
...
2022-12-31 closing balances  ; clopen:2023
...
```
```journal
; 2023.journal
2023-01-01 opening balances  ; clopen:2023
...
```

Now, assuming a combined journal like:

```journal
; all.journal
include 2021.journal
include 2022.journal
include 2023.journal
```

The `clopen:` tag can exclude all but the first opening transaction.
To show a clean multi-year checking register:
```shell
$ hledger -f all.journal areg checking not:tag:clopen
```

And the year values allow more precision.
To show 2022's year-end balance sheet:
```shell
$ hledger -f all.journal bs -e2023 not:tag:clopen=2023
```
