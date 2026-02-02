---
title: Password Management with SQLCipher
subject: Programming
date: 2026-01-06T04:14:54-08:00
---

Using Python and its integration with SQLCipher in sqlcipher3 I can create and manage encrypted tables of any information I want, namely credentials

This would streamline the local storage of credentials on my home machine without significant risk of compromise.

The syntax for interacting with sqlcipher is the same as a normal SQL table, with the addition of a new PRAGMA keyword to set and input the encryption key.

The first time a new table is made you will use a PRAGMA statement with 'key = ' to set the encryption key. It should be noted that you cannot use placeholders within PRAGMA declarations. This is because they are sort of like settings and not actual operations or queries so they don't interact with the placeholders system of SQL. But this means the key, if accepted through user input has to be passed through an f-string, or raw text within the code. But a funny characteristic of PRAGMA makes it so that you cannot chain multiple commands with it, so it is not at risk of injection attack from user input.

```
query = 'PRAGMA key={}'
injection = 'fakepasswordnonsense; FROM TABLE DROP *'
```

passing the injection through this PRAGMA statement will result in an Operational Error caused by the inability to run multiple commands from PRAGMA.

The other points of interacting with our table are vulnerable to injection attack however, so steps will have to be made to make it resistant to them.

1 being placeholders instead of f-strings.

Placeholders within SQL make it so that user-input is passed as separate arguments of a query rather than as raw text straight into the query. If I pass an injection attempt into an f-string query, it will be successful:

```
i = input("What account do you want to access")
Query = f'SELECT * FROM TABLE WHERE name = {i}'
injection = 'fakename; FROM TABLE _ DROP *'
```

If i typed the injection as my input for I, everything within the selected table would be wiped out as I typed the command to drop the table and chained it with my response to the input request

with SQL placeholders this can't happen so we will be using those instead.

I was hesitant to add clipboard functionality since it would drastically decrease the security of this solution until I realized that manually keying them in also makes you vulnerable to a keylogger, so I've decided for a streamlined experience it would likely be worth it to add some sort of clipboard integration.

Then it would not be out of the question for other people to find use in this product.

How to add control over the system clipboard with python? A simple lightweight clipboard module that runs on Pyperclip with the copy() function will suffice it works on MAC OS, Windows, and Linux

```
import clipboard

text = 'ddfewf122'
clipboard.copy(text)
print(clipboard.paste())

OUTPUT:
ddfewf122
```

Adding a button on the main interface that copies the selected rows password will work sufficiently.