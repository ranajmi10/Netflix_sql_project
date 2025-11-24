# Netflix Movies and TV Shows Data Analysis using sql

![Netflix Logo](https://github.com/ranajmi10/Netflix_sql_project/blob/main/logo.png)

## Objectives 
▪ Analyze the distribution of content types (movies vs TV shows).

▪ Identify the most common ratings for movies and TV shows.

▪ List and analyze content based on release years, countries, and durations.

▪ Explore and categorize content based on specific criteria and keywords.

## Dataset
The data for this project is sourced from the Kaggle dataset:

Dataset Link : [Netflix Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)

## Schema 

```sql
CREATE TABLE netflix
(
show_id varchar(6),
type varchar(10),
title varchar(150),
director varchar(208),
casts varchar(1000),
country varchar(150),
date_added varchar(50),
release_year INT,
rating varchar(10),
duration varchar(15),
listed_in varchar(100),
description varchar(250) 
);
