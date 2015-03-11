---
layout: post
title: Script is working
---

So I've been messing around with Mac development for the last little while. I got a copy of "Beginning Mac OS X Programming" and I'm reading it. I also got my iCal script to work, here it is

<!--more-->

```
tell application "iCal"
 activate
 set the my_summary to ""
 set the calendar_list to every calendar
 repeat with i from 1 to the count of the calendar_list
  set the the_calendar to the item i of the calendar_list
  set the event_list to (every event of the the_calendar)
  repeat with j from 1 to the count of event_list
   set the_event to the item j of the event_list
   set my_summary to my_summary & return & return & my summerise_event(the_event)
  end repeat
 end repeat
 set the clipboard to my_summary
end tell


on summerise_event(the_event)
 tell application "iCal"
  set the event_properties to the properties of the_event
  set this_summary to the summary of the event_properties
  set this_location to the location of the event_properties
  if this_location is missing value then set this_location to "No Location"
  set start_date to short date string of (get start date of the event_properties)
  set end_date to short date string of (get end date of the event_properties)
  set event_description to the description of the event_properties
  if event_description is missing value then
   set event_description to "No Notes"
  end if
  set event_summary to "Summary: " & this_summary & return & "Location: " & this_location & return & "Start Date: " & start_date & return & "End Date: " & end_date & return & "Notes: " & event_description & return
 end tell
 return the event_summary
end summerise_event

```

It's a really simple script that get the info about your events from iCal and puts them in the clipboard for you to past wherever.


