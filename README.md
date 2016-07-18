# AirlineDelays

## Problem Motivation

USDOT reports that in 2015 alone, nearly 900 million passengers have been flown through US airports. Of these, 12 are among the world's 30 busiest airports. It is almost an understatement to say that air travel is a ubiquitous part of American life. 

Most of us rely on air travel during holiday periods, which can be a great time to rejuvenate or spend some quality time with family and friends. However, a delayed flight and/or a missed connection can wreak havoc on our plans, bookings and reservations. On average, roughly 20% of flights are delayed or cancelled, which is significant enough that it should be explored further. 

As a passenger, I would like to have information about the best time to fly, airport to fly from and even airlines to avoid so as to minimize my chance of encountering a delay. Ideally, I the end product would be an app that provides me information about the chance that a flight gets delayed, and if so by how long, as I am booking a flight. This would allow me to make more informed decisions about the flight option that I choose to book. 

Specifically for this prototype, I choose to focus only on flights close to holiday periods. During these periods, air traffic typically peaks, as does the number of delays.
 
## Data Sources

I obtained air traffic data from the [Bureau of Transportation Statistics] (http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time), for the time period from December 2010 to May 2016. I obtained this data only for months which are around holiday periods. I also only obtained the data from the states of GA, CA and IL, as I focussed my study on 4 of the busiest airports in USA(ATL,SFO,LAX,ORD).

To get the holiday periods, I used [HolidayAPI] (https://holidayapi.com/), which returned all holidays in US when requested.

## Analysis

#### Exploratory analysis

First, I sought to explore trends in the data during holiday periods. To do this, I decided to characterize each holiday period into 5 parts. This is best explained with an example:

Class -2: 2 days before a holiday period. E.g. Thursday before MLK day or Memorial Day, Tuesday prior to Thanksgiving Day
Class -1: 1 days before a holiday period. E.g. Friday before MLK day or Memorial Day, Wednesday prior to Thanksgiving Day
Class 0: holiday period. E.g. Saturday-Sunday before MLK day or Memorial Day, Thursday-Friday-Saturday of Thanksgiving Day
Class 1: 1 day after holiday period e.g. MLK day or Memorial Day, or Sunday of Thanksgiving when I expect most to fly back
Class 2: 2 days after holiday period - Day after class 12

I did a simple aggregation of times to 
Morning: 5am-Noon
Afternoon: Noon-7pm
Evening: 7pm-2am

I found that in general, the fraction of flights delayed accumulates through the days. That said, the fraction of flights delayed is significantly lower during a holiday period (class 0). 

Moreover, I observed that given that a flight is delayed, the amount that it delayed doesn't appear to depend on the day or time of the flight.

#### Machine Learning

I used a logistic regression classify flights by whether they will be delayed or not. I found that the recall was very high, which is suitable in this case since I want to correctly classify as many delays as possible. I used this to study the predicted probability that the flights would be delayed at a given airport on a given day at a given time. Since the classifier was inaccurate however, the probability value itself is not as important as it's relative value across the airports. This is illustrated in the plot DelayProp.png.

I then used a linear regression to predict the amount of time that a flight will be delayed on a given day at a given time at an airport, given that the flight is delayed. Surprisingly, even though the trend seemed to be constant across all the days, linear regression predicted a spike in delay times at the end of the holiday period, which requires further exploration.

## Future Work

Future enhancements include (in order of importance)

1. Getting data for more years
2. Classifying "Days within holiday period" for the rest of the holiday periods. This requires a bit more effort to define what is a holiday period if a holiday (e.g. 4th of July) falls on a Wednesday. More data exploration will be required.
3. Getting data for more airports
4. Classifying over more features including (but not restricted to)
  Finer time periods (Early morning, late morning, early evening, etc)
  Airline type
  Expected flight distance
  Destination airport  
  Of course, these would only be useful if we obtained more data and considered more time periods.