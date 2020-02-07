---
layout: post
title: NYC MTA Station Advertising

---

### Premise

We were tasked with planning an advertising strategy around NYC subway stations.  The end result was a recommendation of NYC stations near which promoters should focus their efforts.

The ultimate goal was to raise awareness and participation for a fundraising event promoting women in technology.

### Strategy

NYC's subway usage data is readily available through the [MTA website.](http://web.mta.info/developers/turnstile.html "MTA Turnstile Data") This data was readily used to predict stations with high marketing potential.

In choosing stations to recommend, we evaluated the following:
- Volume (how many people go through the station every day?)
- Time of day
- Weekdays or weekends
- Proximity of technology companies

### Results

<img src="/img/weekdays.png"/>  

Intuitively, public transit experiences higher volumes on weekdays than weekends, as people are commuting to/from work.  

The peak windows for overall volume over the course of the day were during morning commutes (7-9AM) and evening commutes (4-6PM) (not shown).

<img src="/img/tech_stops.png"/>  

Above shows the final recommendations for stations during morning weekday commutes.  The black marks correspond to technology companies in the area, while the red pins are the stations themselves.

###### Recommendations
- 47-50 STS ROCK  
- GRD CENTRL-42 ST  
- 34 ST-HERALD SQ  
- 23 ST  
- ASTOR PLACE  
- 18TH ST  
- LEXINGTON AV/53  
- TIMES SQ-42 ST  
- 42 ST-BRYANT PK  
- WALL ST  

### Technologies

#### Python
- pandas
- geopy
- matplotlib  

#### Google Maps API  

#### StackOverflow.com  

### Thoughts

- Data processing is much faster in pandas than it is in loops and dictionaries.
- Perfectionism will get you nowhere.
- Bare minimalism will get you nowhere either.
