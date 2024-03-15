---
layout: post
author: Alan Nightingale
title: "How I Use AppleScript to Paste Dates and Times"
date: 2024-03-15 04:00:00 -0500
category: sharing
tags: mac macOS AppleScript 
---

In my day-to-day tasks I write the date and time a lot.

I've been a happy [Rocket](https://matthewpalmer.net/rocket/) user for years. In Rocket Pro, I created custom snippets using the [built-in variables](https://matthewpalmer.net/rocket/help.html#snippet-variables) to insert the date (e.g., 2024-03-15) or the date and time (e.g., 2024-03-15 04:00:00) in whatever I'm working on.

For writing the date generally, this works great. However, I also date and time stamp files and manually edit the output to a more friendly convention for filesystems. When I write on this blog I manually edit the date and time to a [format that Jekyll expects](https://jekyllrb.com/docs/front-matter/#predefined-variables-for-posts) when posting. This manual step is enough to break my flow. I'd rather just insert what I need and move on.

I looked into how I can insert the date as `YYYY-MM-DD_HHMM` for files and as `YYYY-MM-DD HH:MM:SS +/-TTTT` for Jekyll blog posts.

Rocket doesn't have a more granular date and time segments. macOS Text Replacements doesn't allow for variables. BetterTouchTool, Keyboard Maestro, and TextExpander would all work but seem overpowered for my current text replacement needs. I do fine with macOS Text Replacements and Rocket (other than these more specific date and time use cases).

I decided on AppleScript and [FastScripts](https://redsweater.com/fastscripts/) for my solution. I use AppleScript enough to keep the Script menu in my menu bar. At a glance AppleScript seems to give me the level of granularity in a date and time that I want. I've used the trial of FastScripts before and enjoyed it, and using a FastScript keyboard shortcut allows me to quickly insert the date and time without breaking my concentration. I can also see myself using FastScripts more in the future.

Here are my AppleScripts.

```AppleScript
----------------------------------------
--
-- Paste Date
--
-- About: Pastes the date and time in YYYY-MM-DD format.
-- Author: Alan Nightingale
-- Created: 2024-03-08
-- Updated: n/a
--
----------------------------------------

-- Get the specific date and time pieces
set currentDate to current date
set currentYear to year of currentDate
set currentMonth to month of currentDate as integer -- as integer is needed to return 3 instead of March
set currentDay to day of currentDate

-- Format the month with a leading zero if necessary
if currentMonth < 10 then
  set currentMonth to "0" & currentMonth
end if

-- Format the day with a leading zero if necessary
if currentDay < 10 then
  set currentDay to "0" & currentDay
end if

-- Build the date string
set formattedDate to currentYear & "-" & currentMonth & "-" & currentDay as string

-- Copy
set the clipboard to formattedDate

-- Paste
tell application "System Events" to keystroke "v" using {command down}
```

```AppleScript
----------------------------------------
--
-- Paste Date and Time for Files
--
-- About: Pastes the date and time in YYYY-MM-DD_HHMM format.
-- Author: Alan Nightingale
-- Created: 2024-03-07
-- Updated: n/a
--
----------------------------------------

-- Get the specific date and time pieces
set currentDate to current date
set currentYear to year of currentDate
set currentMonth to month of currentDate as integer -- as integer is needed to return 3 instead of March
set currentDay to day of currentDate
set currentHour to hours of currentDate
set curentMinute to minutes of currentDate

-- Format the month with a leading zero if necessary
if currentMonth < 10 then
  set currentMonth to "0" & currentMonth
end if

-- Format the day with a leading zero if necessary
if currentDay < 10 then
  set currentDay to "0" & currentDay
end if

-- Format the hour with a leading zero if necessary
if currentHour < 10 then
  set currentHour to "0" & currentHour
end if

-- Format the minute with a leading zero if neccessary
if curentMinute < 10 then
  set curentMinute to "0" & curentMinute
end if

-- Build the date string
set formattedDate to currentYear & "-" & currentMonth & "-" & currentDay & "_" & currentHour & curentMinute as string

-- Copy
set the clipboard to formattedDate

-- Paste
tell application "System Events" to keystroke "v" using {command down}
```

```AppleScript
----------------------------------------
--
-- Paste Date and Time for Jekyll
--
-- About: Pastes the date and time in YYYY-MM-DD HH:MM:SS +/-TTTT format.
-- Author: Alan Nightingale
-- Created: 2024-03-08
-- Updated: n/a
--
----------------------------------------

-- Get the specific date and time pieces
set currentDate to current date
set currentYear to year of currentDate
set currentMonth to month of currentDate as integer -- as integer is needed to return 3 instead of March
set currentDay to day of currentDate
set currentHour to hours of currentDate
set currentMinute to minutes of currentDate
set currentSecond to seconds of currentDate
set currentOffset to do shell script "date +%z"
-- TODO add offset

-- Format the month with a leading zero if necessary
if currentMonth < 10 then
  set currentMonth to "0" & currentMonth
end if

-- Format the day with a leading zero if necessary
if currentDay < 10 then
  set currentDay to "0" & currentDay
end if

-- Format the hour with a leading zero if necessary
if currentHour < 10 then
  set currentHour to "0" & currentHour
end if

-- Format the minute with a leading zero if neccessary
if currentMinute < 10 then
  set currentMinute to "0" & currentMinute
end if

-- Format the second with a leading zero if neccessary
if currentSecond < 10 then
  set currentSecond to "0" & currentSecond
end if

-- Build the date string
set formattedDate to currentYear & "-" & currentMonth & "-" & currentDay & " " & currentHour & ":" & currentMinute & ":" & currentSecond & " " & currentOffset as string

-- Copy
set the clipboard to formattedDate

-- Paste
tell application "System Events" to keystroke "v" using {command down}
```

The scripts are also available as GitHub Gists:

- [Paste Date.scpt](https://gist.github.com/ajnx/c3967ad0a43ee392615f81278da5c5ad)
- [Paste Date and Time for Files.scpt](https://gist.github.com/ajnx/b5b331d6b5a5e9403651335cfefba228)
- [Paste Date and Time for Jekyll.scpt](https://gist.github.com/ajnx/00022876d1ef4d43d1782f0200390737)

I've been using these for about a week now and it's been pretty slick. It's faster than typing the formatted date out by hand. It speeds up what I'm working on and keeps my brain in a flow state. They've been useful to me already and hopefully they're useful to you too.
