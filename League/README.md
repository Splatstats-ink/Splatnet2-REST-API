# League data

The method of requesting league data is via the base URL and 2 additional paths.

The response will be in the form of JSON that can be iterated through as a list/array.

But you may want to clean up the JSON with popper indenting before you dive into working with it. As is comes out very messy. In python you can do this by:

```python
file = 'example.json'
with open(file) as json_data:
    parsed = json.load(json_data)
    data = json.dumps(parsed, indent=4, sort_keys=True)
```

Base URL =`https://app.splatoon2.nintendo.net/api/league_match_ranking/`

## Path 1

The first path used specifies the date of the league data and the grouping.

It's looks something like : `18071500T`

This can be split up into 5 distinct parts, **that are typically 2 digits :**

- Year *eg:*`18`
- Month *eg:*`07`
- Day *eg:*`15`
- Time *eg:* `00`
- Grouping *eg:*`T`

#### Year:
This is 2 digits in the path and tell splatnet what year you want to request data from.

So 2018 will be `18` and 2017 would be `17`

#### Month:
This is 2 digits in the path and tell splatnet what month you want to request data from.

**Note: Trailing zero is required**

Month | Digits
------|-------
January | `01`
February | `02`
March | `03`
April | `04`
May | `05`
June | `06`
July | `07`
August | `08`
September | `09`
October | `10`
November | `11`
December | `12`

#### Day:
This is 2 digits in the path and tell splatnet what day you want to request data from.

This is the calendar day in the month.

#### Time:
This is 2 digits in the path and tell splatnet what time you want to request data from.

All time used in Splatnet 2 is in **UTC** in 2 hour intervals and are as follows:

**Note: Trailing zero is required**

Time(UTC) | Digits
----------|-------
00:00-01:59 | `00`
02:00-03:59 | `02`
04:00-05:59 | `04`
06:00-07:59 | `06`
08:00-09:59 | `08`
10:00-11:59 | `10`
12:00-13:59 | `12`
14:00-15:59 | `14`
16:00-17:59 | `16`
18:00-19:59 | `18`
20:00-21:59 | `19`
22:00-23:59 | `22`

#### Grouping:
This is 1 character in the path and tell splatnet what grouping you want to request data from.

It can:

- Twin/Pair : `P`
- Team/Quad : `T`

## Path 2
This path is used to specify the region of which you want data about.

The current regions usable are:

Region(s) | code
----------|-----
North America / Oceania | `US`
Europe | `EU`
Japan | `JP`
All Regions | `ALL`

## Example
In this example I want to get data from **June 15th 2018** at **20:00-21:59(UTC)** in a **Pair** from **ALL** regions combined

Path one will be `18061520P`:
`18` from 2018
`06` from June
`15` from 15th
`20` from 20:00-21:59(UTC)
`P` from Pairs

Path two would be `ALL`

So when combined you should have the URL : `https://app.splatoon2.nintendo.net/api/league_match_ranking/18061520P/ALL`

*The result from this URL can be found in example.json of this repo in the same directory as this readme.md*


## Errors
Splatnet 2 may throw up an error sometimes that looks like This

```json
{"code": "NOT_FOUND_ERROR", "message": "api_error_not_found_error"}
```

This typically means your path 1 isn't correct, however this can also happen in the following cases:

- In a Splatfest (No league data exists)
- No players in the regions plays at that time (Common in Europe)
- Incorrect path 1 formatting
