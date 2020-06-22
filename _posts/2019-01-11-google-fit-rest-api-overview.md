---
title: "Google Fit REST API - Overview"
date: "2019-01-11"
---

## Overview

In this fast paced world and hectic life schedule, fitness is utmost important. With the technology advances, we can track our fitness activities. There are number of fitness bands and apps available in the market and app stores. We can track all our daily activities by using these fitness bands or apps. As a developer, we want to go one step ahead and start programming with our fitness data so that we can use it in any applications, mold it further.

In this article, we are going to explore Google Fitness API and its support to start programming against it.

## Why Google Fit?

As I mentioned there are number of fitness bands and apps available. There are few reasons for me to go for Google fitness:

1. It’s free
2. It does not need any additional band to be tied up on the wrist

More information about Google Fit can be found here ([https://www.google.com/fit/](https://www.google.com/fit/))

## Monitor Your Google Fit Data

Once the app is downloaded to mobile, it keep tracks of all your activities (provided you are carrying your mobile during the activities of running, jogging, walking, etc.). The Google fit data can be seen over the web at [https://fit.google.com/](https://fit.google.com/)

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-308.png)

You can customize the page to add your own activities and log your weight.

**How to sync the data?**

The data is recorded on your phone by app. Use the Google account sync option to sync it with web.

Below are the steps for Android users:

1. Go to the Settings > Accounts
2. Select Google
3. Sync On the “Google Fit data”

## Google Fit REST API

Google Fit REST enables to store and access user data in fitness store. There are few keywords to understand for Google Fit REST API.

- **Data Source:** Represents unique source of sensor data. Data sources expose raw data generated from hardware sensors. It can also expose derived data by transforming or merging multiple data sources. It uniquely identifies the data based on hardware device and app recorded or transformed the data.
- **Dataset:** Represents set of points from particular data source. It represents data at fixed boundaries.
- **Data Point:** Represents sample from particular data source. Data Point holds value for each field per timestamp.
- **Session:** Represents time interval. It allows to query the data. Start time and end time for sessions is controlled by applications and is used to represent user friendly information like walking, running, bike ride, etc.
- **Data Type:** Represents the schema for the data being recorded. It only defines representation and format of data. As an example com.google.step\_count.delta data type represents step count as delta between start and end time, whereas com.google.step\_count.cumulative as sum of steps between start and end time.

## Fitness Data Types

The fitness data types are available under **com.google** namespace. A data type can represent

- A particular reading (includes only timestamp)
- An aggregate over time interval (also include timestamp for start of interval)

You may also create your own data types, if needed.

There are 3 types of Data types

**Public Data Types**

These are the standard data types provided by platform. Below are few often used public data types:

**Data Type Name**

**Description**

com.google.calories.expended

Total calories expanded over given time interval

com.google.distance.delta

Distance covered since last reading

com.google.heart\_rate.bpm

Heart rate

com.google.height

User height (in meters)

com.google.weight

User weight (in kg)

com.google.step\_count.delta

New steps since last reading

**Custom Data Types**

We can create our own custom data types to store the fitness data. Custom data types are created per application. Data type created in one application will not be available to use in other applications. To handle this scenario, use Shareable data types.

**Shareable Data Types**

These are custom data types approved by Google to be shared on Google Fit. To create new shareable data type, send an email to [google-fit-shareable@google.com](mailto:google-fit-shareable@google.com) with description of requested data type and explaining how it will be useful to other users and developers.

## Summary

Google Fit provides the REST APIs to store and access user data in fitness store. Google Fit has predefined data types to represent the fitness data. Using the data types, information can be shared within and across applications.

This content was originally posted [here](https://www.c-sharpcorner.com/article/google-fit-rest-api-overview-part-1/).
