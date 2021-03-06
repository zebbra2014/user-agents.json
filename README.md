# user-agents.json
This is a JSON file of User-Agent regular expressions,
and an implementation which can parse this file and determine the parameters of the user-agent.

By separating the rules from the logic which processes the rules,
the rules file can be easily and quickly updated, and the logic can be configured
to download the rules file from a central location at regular intervals.

## Implementations
At present only a PHP implementation is provided, but implementations would not be difficult to create
in other languages (pull requests welcome).

This current implementation does not download the file from a central server, but this would not be
difficult to implement either in the calling code or by extending the implementation (pull requests welcome).

## Input
The input should be a full user agent string

## Output fields
Output fields are as follows:

| Field | Purpose                  | Examples                                |
|-------|--------------------------|-----------------------------------------|
| dc    | Device category          | Mobile, Tablet, Desktop                 |
| on    | Operating system name    | Windows, Macintosh, Linux, Android, iOS |
| ov    | Operating system version | 5.0, 5.1, 10.2                          |
| ot    | Operating system title   | Windows XP, Windows 7                   |
| bn    | Browser name             | Chrome, Firefox, Safari                 |
| bv    | Browser version          | 73.0.3683.75, 60.0                      |
| bot   | Name of robot            | Google, Bing                            |

Note that fields will not be set if the system cannot determine the value.

## Automatically fetching data
It's a good idea to auto-update the JSON rules from time to time, so that new
devices and browsers will be recognised.

The URL for fetching data is:
```
https://raw.githubusercontent.com/Karmabunny/user-agents.json/master/data/user-agents.json
```

This fetch can be done easily using cron/cURL or server-side scripting code.

## Adding rules
Matching begins with an empty state object.

Rules are a PCRE regular expression. If the rule matches, then the parameters for that rule are applied to the
current state object.

Parameters can contain subexpressions in the form $1, $2, etc.

Rules can be nested. If a rule matches, all of its sub-rules are tested as well.
