---
title: Highlights in R 4.3.0
author: Mikko Marttila
date: '2023-04-23'
slug: highlights-in-r-4-3-0
categories:
  - r
tags:
  - note
  - news
---

R version 4.3.0 was released on Friday. Here are my highlights from the [release notes](https://cloud.r-project.org/bin/windows/base/NEWS.R-4.3.0.html).

<!--more-->

## The pipe evolves

> As an experimental feature the placeholder `_` can now also be used in the rhs of a forward pipe `|>` expression as the first argument in an extraction call [...]

It's fantastic to see base R's pipe evolve further. This is something that had bothered
me a lot personally, not being able to extract for example list elements in a pipe
chain easily. But now we (experimentally) can:


```r
lm(mpg ~ wt, data = mtcars) |> _$coef[2] |> round(2)
```

```
##    wt 
## -5.34
```

## Date ergonomics

> `as.POSIXct(<numeric>)` and `as.POSIXlt(.)` (without specifying origin) now work. So does `as.Date(<numeric>)`.

If you have ever had to convert epoch numbers into dates or datetimes (maybe
because your dates lost their class for some reason, or otherwise),
this should save you the trouble of remembering (and typing out) that
the Unix epoch starts on January 1, 1970:


```r
as.Date(0)
```

```
## [1] "1970-01-01"
```

> The `as.POSIXlt(<POSIXlt>)` and `as.POSIXct(<POSIXct>)` default methods now do obey their `tz` argument, also in this case.

I was very recently confused by these functions ignoring `tz`, so I'm pleased to see this now works:


```r
as.POSIXct(Sys.time(), "America/New_York")
```

```
## [1] "2023-04-23 06:34:24 EDT"
```

## Logical safety

> Calling `&&` or `||` with LHS or (if evaluated) RHS of length greater than one is now always an error [...]

This has been a while coming, but should now clearly catch a class of errors
caused by accidentally using a vector in an if statement:


```r
if (LETTERS == "A" || LETTERS == "B") {}
```

```
## Error in LETTERS == "A" || LETTERS == "B": 'length = 26' in coercion to 'logical(1)'
```

## Upcoming OOP

> The `@` operator is now an S3 generic. Based on contributions by Tomasz Kalinowski in [PR#18482](https://bugs.r-project.org/show_bug.cgi?id=18482).

This may seem a bit mysterious at first glance. However, it's one of the
steps paving the way for the upcoming [S7 object-oriented programming system](https://rconsortium.github.io/OOP-WG/).
S7 is looking great, and I'm glad to see progress being made for its release
with this and other updates.

## Bonus

> Added new unit prefixes "R" and "Q" for abbreviating (unrealistically large) sizes beyond 10271027 in `standard = "SI"`, thanks to Henrik Bengtsson's [PR#18435](https://bugs.r-project.org/show_bug.cgi?id=18435).

I don't know if R ended up being the first programming language to implement these
new SI prefixes (as aspired to in Henrik's PR), but I found this delightful.


```r
print(structure(4.3e27, class = "object_size"), units = "auto", standard = "SI")
```

```
## 4.3 RB
```

## Fin

And that's it from my prespective. These were just my highlights, and there is
of course a lot more to be seen in the [full release notes](https://cloud.r-project.org/bin/windows/base/NEWS.R-4.3.0.html).
My thanks go out to R Core and all the contributors for continuing to make R
better for all of us with yet another great release.
