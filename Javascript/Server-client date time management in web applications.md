# [Server-client date time management in web applications](https://codinginthetrenches.com/2014/07/25/server-client-date-time-management-in-web-applications/)

Storing time information using [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time)is the best way to go when building a multi-time-zone system. UTC has several advantages compared to using a specific time zone, one of the most important being that it is the international standard time for civil usage. Additionally, it is easy to convert from a time stored as UTC to any time zone since time zones are defined as offsets of hours and minutes from UTC. There are other difficulties presented by trying to store dates and times centrally using a time zone other than UTC



# [Working with Time Zones](https://www.w3.org/TR/timezone/)

### [What Is a Time Zone?]()

A time zone is a set of rules for determining the local observed time (wall time) as it relates to incremental time (as used in most computing systems) for a particular geographical region.

### [What about Daylight Saving (Summer) Time?]()

More recently, the concept of "Daylight Saving Time" (DST) or "Summer Time" was adopted as a way of allowing people more sunlight hours in the evening. DST varies from country to country (not to mention locality-to-locality) and often has "special one-off" changes to accommodate special events. Not all regions observe DST: usually those closer to the equator do not need it.



### [How is a Time Zone Defined?]()

**UTC Offset** All time zones have, as their basis, an offset from UTC. It is the difference (positive or negative) between when a given time event is observed in UTC and in local time. If all time zones used one-hour offsets, there would be 25 world-wide time zones, ranging between 12 hours before UTC to 12 hours following UTC. However, there are some that use half-hour or even quarter-hour offsets (or even some odd offsets). In addition, some time zones fall outside a single-day 