---
title: "Create hashtable using two different variables in PowerShell"
date: 2019-07-21T22:37:57+05:30
draft: false
tags: ["hashtable", "PowerShell", "how-to"]
---
Here is how we can combinine two variables into hashtable,
<!--more-->

```PowerShell
PS > $value1 = "Android", "iOS", "Tizen"
PS > $value2 = "Google", "Apple", "Samsung"
PS > #Use GetEnumerator()
PS > $value1E = $value1.GetEnumerator()
PS > $value2E = $value2.GetEnumerator()
PS > #combine $value1E and $value2E into hashtable
PS > $table = @{}
PS > while($value1E.MoveNext() -and $value2E.MoveNext()){$table.Add($value1E.current,$value2E.current)}
PS > $table

Name                           Value
----                           -----
Tizen                          Samsung
Android                        Google
iOS                            Apple

PS >
```

