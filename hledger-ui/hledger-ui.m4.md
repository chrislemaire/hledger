% hledger-ui(1)
% _author_
% _monthyear_

_notinfo_({{
# NAME
}})

hledger-ui - robust, friendly plain text accounting (TUI version)

_notinfo_({{
# SYNOPSIS
}})

`hledger-ui [OPTIONS] [QUERYARGS]`\
`hledger ui -- [OPTIONS] [QUERYARGS]`

_notinfo_({{
# DESCRIPTION
}})

This manual is for hledger's terminal interface, version _version_.
See also the hledger manual for common concepts and file formats.

_hledgerdescription_

_web_({{
<div class="screenshots-right">
<a href="/images/hledger-ui/hledger-ui-sample-acc2.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-sample-acc2.png" title="Accounts screen with query and depth limit" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-sample-acc.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-sample-acc.png" title="Accounts screen" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-sample-acc-greenterm.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-sample-acc-greenterm.png" title="Accounts screen with greenterm theme" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-sample-txn.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-sample-txn.png" title="Transaction screen" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-sample-reg.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-sample-reg.png" title="Register screen" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-bcexample-acc.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-bcexample-acc.png" title="beancount example accounts" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-bcexample-acc-etrade-cash.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-bcexample-acc-etrade-cash.png" title="beancount example's etrade cash subaccount" height="180"/></a>
<a href="/images/hledger-ui/hledger-ui-bcexample-acc-etrade.png" class="highslide" onclick="return hs.expand(this)"><img src="/images/hledger-ui/hledger-ui-bcexample-acc-etrade.png" title="beancount example's etrade investments, all commoditiess" height="180"/></a>
</div>
}})

hledger-ui is hledger's terminal interface, providing an efficient full-window text UI
for viewing accounts and transactions, and some limited data entry capability.
It is easier than hledger's command-line interface, and
sometimes quicker and more convenient than the web interface.

Like hledger, it reads _inputfiles_
For more about this see hledger(1), hledger_journal(5) etc.

Unlike hledger, hledger-ui hides all future-dated transactions by default.
They can be revealed, along with any rule-generated periodic transactions,
by pressing the F key (or starting with --forecast) to enable "forecast mode".

# OPTIONS

Note: if invoking hledger-ui as a hledger subcommand, write `--` before options as shown above.

Any QUERYARGS are interpreted as a hledger search query which filters the data.

`-w --watch`
: watch for data and date changes and reload automatically

`--theme=default|terminal|greenterm`
: use this custom display theme

`--menu`
: start in the menu screen

`--all`
: start in the all accounts screen

`--bs`
: start in the balance sheet accounts screen

`--is`
: start in the income statement accounts screen

`--register=ACCTREGEX`
: start in the (first) matched account's register screen

`--change`
: show period balances (changes) at startup instead of historical balances

`-l --flat`
: show accounts as a flat list (default)

`-t --tree`
: show accounts as a tree

 hledger input options:

_inputoptions_

hledger reporting options:

_reportingoptions_

hledger help options:

_helpoptions_

A @FILE argument will be expanded to the contents of FILE,
which should contain one command line option/argument per line.
(To prevent this, insert a `--` argument before.)

# MOUSE

In most modern terminals, you can navigate through the screens with a
mouse or touchpad:

- Use mouse wheel or trackpad to scroll up and down
- Click on list items to go deeper
- Click on the left margin (column 0) to go back.

# KEYS

Keyboard gives more control.

`?` shows a help dialog listing all keys.
(Some of these also appear in the quick help at the bottom of each screen.)
Press `?` again (or `ESCAPE`, or `LEFT`, or `q`) to close it.
The following keys work on most screens:

The cursor keys navigate:
`RIGHT` or `ENTER` goes deeper,
`LEFT` returns to the previous screen,
`UP`/`DOWN`/`PGUP`/`PGDN`/`HOME`/`END` move up and down through lists.
Emacs-style (`CTRL-p`/`CTRL-n`/`CTRL-f`/`CTRL-b`)
and VI-style (`k`,`j`,`l`,`h`) 
movement keys are also supported.
A tip: movement speed is limited by your keyboard repeat rate,
to move faster you may want to adjust it.
(If you're on a mac, the karabiner app is one way to do that.)

With shift pressed, the cursor keys adjust the report period,
limiting the transactions to be shown (by default, all are shown).
`SHIFT-DOWN/UP` steps downward and upward through these standard report period durations:
year, quarter, month, week, day.
Then, `SHIFT-LEFT/RIGHT` moves to the previous/next period.
`T` sets the report period to today.
With the `-w/--watch` option, when viewing a "current" period
(the current day, week, month, quarter, or year),
the period will move automatically to track the current date.
To set a non-standard period, you can use `/` and a `date:` query.

(Mac users: SHIFT-DOWN/UP keys do not work by default in Terminal, as of MacOS Monterey.
You can configure them as follows: 
open Terminal,
press CMD-comma to open preferences,
click Profiles,
select your current terminal profile on the left,
click Keyboard on the right,
click + and add this for Shift-Down: `\033[1;2B`,
click + and add this for Shift-Up:   `\033[1;2A`.
Press the Escape key to enter the `\033` part, you can't type it directly.)

`/` lets you set a general filter query limiting the data shown,
using the same [query terms](hledger.html#queries) as in hledger and hledger-web.
While editing the query, you can use [CTRL-a/e/d/k, BS, cursor keys](http://hackage.haskell.org/package/brick-0.7/docs/brick-widgets-edit.html#t:editor);
press `ENTER` to set it, or `ESCAPE`to cancel.
There are also keys for quickly adjusting some common filters like account depth and transaction status (see below).
`BACKSPACE` or `DELETE` removes all filters, showing all transactions.

As mentioned above, by default hledger-ui hides future transactions -
both ordinary transactions recorded in the journal, and periodic
transactions generated by rule. `F` toggles forecast mode, in which
future/forecasted transactions are shown.

`ESCAPE` resets the UI state and jumps back to the top screen,
restoring the app's initial state at startup.
Or, it cancels minibuffer data entry or the help dialog.

`CTRL-l` redraws the screen and centers the selection if possible
(selections near the top won't be centered, since we don't scroll above the top).

`g` reloads from the data file(s) and updates the current screen and any
previous screens. (With large files, this could cause a noticeable pause.)

`I` toggles balance assertion checking.
Disabling balance assertions temporarily can be useful for troubleshooting.

`a` runs command-line hledger's add command, and reloads the updated file.
This allows some basic data entry.

`A` is like `a`, but runs the [hledger-iadd](http://hackage.haskell.org/package/hledger-iadd) tool,
which provides a terminal interface.
This key will be available if `hledger-iadd` is installed in $path.

`E` runs $HLEDGER_UI_EDITOR, or $EDITOR, or a default (`emacsclient -a "" -nw`) on the journal file.
With some editors (emacs, vi), the cursor will be positioned at the current transaction
when invoked from the register and transaction screens, and at the error location (if possible)
when invoked from the error screen.

`B` toggles cost mode, showing amounts in their cost's commodity
(like toggling the [`-B/--cost`](https://hledger.org/hledger.html#b-cost) flag).

`V` toggles value mode, showing amounts' current market value in their
default valuation commodity (like toggling the
[`-V/--market`](https://hledger.org/hledger.html#v-market-value) flag).
Note, "current market value" means the value on the report end date if specified, otherwise today.
To see the value on another date, you can temporarily set that as the report end date.
Eg: to see a transaction as it was valued on july 30,
go to the accounts or register screen,
press `/`,
and add ` date:-7/30` to the query.

At most one of cost or value mode can be active at once.

There's not yet any visual reminder when cost or value mode is active;
for now pressing `b` `b` `v` should reliably reset to normal mode.

`q` quits the application.

Additional screen-specific keys are described below.

# SCREENS

hledger-ui shows several different screens, described below.
It shows the "Balance sheet accounts" screen to start with, except in the following situations:

- If no asset/liability/equity accounts can be detected,
  or if an account query has been given on the command line,
  it starts in the "All accounts" screen.

- If a starting screen is specified with --menu/--all/--bs/--is/--register
  on the command line, it starts in that screen.

From any screen you can press `LEFT` or `ESC` to navigate back to the top level "Menu" screen.

## Menu

The top-most screen.
From here you can navigate to three accounts screens:

## All accounts

This screen shows all accounts (possibly filtered by a query),
and their end balances on the date shown in the title bar
(or their balance changes in the period shown in the title bar, toggleable with `H`).
It is like the `hledger balance` command. 

## Balance sheet accounts

This screen shows asset, liability and equity accounts, if these can be detected (see [account types](/hledger.html#account-types)).
It always shows end balances.
It is like the `hledger balancesheetequity` command.

## Income statement accounts

This screen shows revenue and expense accounts.
It always shows balance changes.
It is like the `hledger incomestatement` command.

All of these accounts screens work in much the same way:

They show accounts which have been posted to by transactions,
as well as accounts which have been declared with an [account directive](#account)
(except for empty parent accounts).

If you specify a query on the command line or with `/` in the app,
they show just the matched accounts, and the balances from matched transactions.

hledger-ui shows accounts with zero balances by default (unlike command-line hledger).
To hide these, press `z` to toggle nonzero mode.

Account names are shown as a flat list by default; press `t` to toggle tree mode.
In list mode, account balances are exclusive of subaccounts, except where subaccounts are hidden by a depth limit (see below).
In tree mode, all account balances are inclusive of subaccounts.

To see less detail, press a number key, `1` to `9`, to set a depth limit.
Or use `-` to decrease and `+`/`=` to increase the depth limit.
`0` shows even less detail, collapsing all accounts to a single total.
To remove the depth limit, set it higher than the maximum account depth, or press `ESCAPE`.

`H` toggles between showing historical balances or period balances (on the "All accounts" screen).
Historical balances (the default) are ending balances at the end of the report period,
taking into account all transactions before that date (filtered by the filter query if any),
including transactions before the start of the report period. In other words, historical
balances are what you would see on a bank statement for that account (unless disturbed by
a filter query). Period balances ignore transactions before the report start date, so they
show the change in balance during the report period. They are more useful eg when viewing a time log.

`U` toggles filtering by [unmarked status](hledger.html#status),
including or excluding unmarked postings in the balances.
Similarly, `P` toggles pending postings,
and `C` toggles cleared postings.
(By default, balances include all postings;
if you activate one or two status filters, only those postings are included;
and if you activate all three, the filter is removed.)

`R` toggles real mode, in which [virtual postings](hledger.html#virtual-postings) are ignored.

Press `RIGHT` to view an account's register screen,
Or, `LEFT` to see the menu screen.

## Register

This screen shows the transactions affecting a particular account, like a check register.
Each line represents one transaction and shows:

- the other account(s) involved, in abbreviated form.
  (If there are both real and virtual postings, it
  shows only the accounts affected by real postings.)

- the overall change to the current account's balance;
  positive for an inflow to this account, negative for an outflow.

- the running historical total or period total for the current account, after the transaction.
This can be toggled with `H`.
Similar to the accounts screen, the historical total is affected by transactions
(filtered by the filter query) before the report start date, while the period total is not.
If the historical total is not disturbed by a filter query, it will be the
running historical balance you would see on a bank register for the current account.

Transactions affecting this account's subaccounts will be included in the register
if the accounts screen is in tree mode,
or if it's in list mode but this account has subaccounts which are not shown due to a depth limit.
In other words, the register always shows the transactions contributing to the balance shown on the accounts screen.
Tree mode/list mode can be toggled with `t` here also.

`U` toggles filtering by [unmarked status](hledger.html#status), showing or hiding unmarked transactions.
Similarly, `P` toggles pending transactions, and `C` toggles cleared transactions.
(By default, transactions with all statuses are shown;
if you activate one or two status filters, only those transactions are shown;
and if you activate all three, the filter is removed.)

`R` toggles real mode, in which [virtual postings](hledger.html#virtual-postings) are ignored.

`z` toggles nonzero mode, in which only transactions posting a nonzero
change are shown (hledger-ui shows zero items by default,
unlike command-line hledger).

Press `RIGHT` to view the selected transaction in detail.

## Transaction

This screen shows a single transaction, as a general journal entry,
similar to hledger's print command and journal format (hledger_journal(5)).

The transaction's date(s) and any cleared flag, transaction code,
description, comments, along with all of its account postings are
shown.  Simple transactions have two postings, but there can be more
(or in certain cases, fewer).

`UP` and `DOWN` will step through all transactions listed in the
previous account register screen.  In the title bar, the numbers in
parentheses show your position within that account register. They will
vary depending on which account register you came from (remember most
transactions appear in multiple account registers). The #N number
preceding them is the transaction's position within the complete
unfiltered journal, which is a more stable id (at least until the next
reload).

## Error

This screen will appear if there is a problem, such as a parse error,
when you press g to reload. Once you have fixed the problem,
press g again to reload and resume normal operation.
(Or, you can press escape to cancel the reload attempt.)


# TIPS

## Watch mode

One of hledger-ui's best features is the auto-reloading `-w/--watch` mode.
With this flag, it will update the display automatically whenever changes
are saved to the data files. 

This is very useful when reconciling. A good workflow is to have
your bank's online register open in a browser window, for reference;
the journal file open in an editor window;
and hledger-ui in watch mode in a terminal window, eg:
```shell
$ hledger-ui --watch --register checking -C
```
As you mark things cleared in the editor,
you can see the effect immediately without having to context switch.
This leaves more mental bandwidth for your accounting.
Of course you can still interact with hledger-ui when needed,
eg to toggle cleared mode, or to explore the history.

Here are some current limitations to be aware of:

Changes might not be detected with certain editors, possibly including 
Jetbrains IDEs, `gedit`, other Gnome applications; or on certain unusual filesystems.
([#1617](https://github.com/simonmichael/hledger/issues/1617),
[#911](https://github.com/simonmichael/hledger/issues/911)).
To work around, reload manually by pressing `g` in the hledger-ui window.
(Or see #1617 for another workaround, and let us know if it works for you.)

CPU and memory usage can sometimes gradually increase, if `hledger-ui --watch` is left running for days.
(Possibly correlated with certain platforms, many transactions, and/or large numbers of other files present).
To work around, `q`uit and restart it,
or (where supported) suspend (`CTRL-z`) and restart it (`fg`).

## Debug output

You can add `--debug[=N]` to the command line to log debug output.
This will be logged to the file `hledger-ui.log` in the current directory.
N ranges from 1 (least output, the default) to 9 (maximum output).

# ENVIRONMENT

**COLUMNS**
The screen width to use.
Default: the full terminal width.

_LEDGER_FILE_

# FILES

Reads _inputfiles_

# BUGS

`-f-` doesn't work (hledger-ui can't read from stdin).

`-V` affects only the accounts screen.

When you press `g`, the current and all previous screens are
regenerated, which may cause a noticeable pause with large files.
Also there is no visual indication that this is in progress.

`--watch` is not yet fully robust. It works well for normal usage, but
many file changes in a short time (eg saving the file thousands of
times with an editor macro) can cause problems at least on OSX.
Symptoms include: unresponsive UI, periodic resetting of the cursor
position, momentary display of parse errors, high CPU usage eventually
subsiding, and possibly a small but persistent build-up of CPU usage
until the program is restarted.

Also, if you are viewing files mounted from another machine, `-w/--watch`
requires that both machine clocks are roughly in step.
