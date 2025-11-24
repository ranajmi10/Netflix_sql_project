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
```
## Business Problems and Solutions

### 1. Count the Number of Movies vs TV Shows

```sql
SELECT
      type,
      COUNT(*) AS total_no_of_content
FROM netflix
GROUP BY type;
```


**Objective:** Determine the distribution of content types on Netflix.

### 2. Find the Most Common Rating for Movies and TV Shows

```sql
SELECT
       type,
       rating
FROM
(
SELECT 
       type ,
       rating,
       COUNT(*),
       RANK() OVER(PARTITION BY type ORDER BY count(*) desc) as ranking
FROM netflix
GROUP BY 1,2
)  AS t1
WHERE ranking =1;
```

**Objective:** Identify the most frequently occurring rating for each type of content.

### 3. List All Movies Released in a Specific Year (e.g., 2020)

```sql
SELECT  *
FROM netflix
WHERE release_year =2020 AND type='Movie';
```

**Objective:** Retrieve all movies released in a specific year.

### 4. Find the Top 5 Countries with the Most Content on Netflix

```sql
SELECT  
      UNNEST(STRING_TO_ARRAY(country,',')) as new_country,
	  COUNT(show_id) AS total_count
FROM netflix
GROUP BY  1 
ORDER BY 2 DESC 
LIMIT 5;
```


**Objective:** Identify the top 5 countries with the highest number of content items.

### 5. Identify the Longest Movie

```sql
SELECT  * 
FROM netflix
WHERE type ='Movie'
AND duration=(SELECT MAX(duration) FROM netflix);
```

**Objective:** Find the movie with the longest duration.

### 6. Find Content Added in the Last 5 Years

```sql
SELECT * 
FROM netflix
WHERE TO_DATE(date_added,'MONTH DD,YYYY')>=CURRENT_DATE - INTERVAL '5 YEARS';
```

**Objective:** Retrieve content added to Netflix in the last 5 years.

### 7. Find All Movies/TV Shows by Director 'Rajiv Chilaka'

```sql
SELECT *
FROM netflix 
WHERE director LIKE '%Rajiv Chilaka%';
```

**Objective:** List all content directed by 'Rajiv Chilaka'.

### 8. List All TV Shows with More Than 5 Seasons

```sql
SELECT * 
FROM netflix
WHERE type ='TV Show'
AND
split_part(duration,' ',1):: numeric >5;
```


### 9. Count the Number of Content Items in Each Genre

```sql
SELECT 
      UNNEST(STRING_TO_ARRAY(listed_in,',')) as genre, 
	  COUNT(show_id) as total_count
FROM netflix
GROUP BY 1 ;
```


**Objective:** Count the number of content items in each genre.

### 10.Find each year and the average numbers of content release in India on netflix. 
return top 5 year with highest avg content release!

```sql
SELECT 
EXTRACT(YEAR FROM TO_DATE(date_added,'Month DD,YYYY')) AS year ,
COUNT(*) AS yearly_content,
ROUND(
COUNT(*)::numeric/(SELECT COUNT(*) FROM netflix WHERE country = 'India'):: numeric * 100,2
) AS avg_content_per_year 
FROM netflix
WHERE country ='India'
GROUP BY 1;
```


**Objective:** Calculate and rank years by the average number of content releases by India.

### 11. List All Movies that are Documentaries

```sql
SELECT * 
FROM netflix
WHERE listed_in ILIKE '%documentaries%';
```


**Objective:** Retrieve all movies classified as documentaries.

### 12. Find All Content Without a Director

```sql
SELECT *
FROM netflix
WHERE director IS NULL;
```
**Objective:** List content that does not have a director.

### 13. Find How Many Movies Actor 'Salman Khan' Appeared in the Last 10 Years

```sql
SELECT *
FROM netflix
WHERE casts ILIKE '%Salman Khan%'
AND
release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

*Objective:** Count the number of movies featuring 'Salman Khan' in the last 10 years.

### 14. Find the Top 10 Actors Who Have Appeared in the Highest Number of Movies Produced in India

```sql
SELECT
 UNNEST(STRING_TO_ARRAY(casts,',')) AS actors,
 COUNT(*) AS total_content
 FROM netflix
 WHERE country ILIKE '%india%'
 GROUP BY  1 
 ORDER BY 2 DESC
 LIMIT 10;
```

**Objective:** Identify the top 10 actors with the most appearances in Indian-produced movies.

### 15. Categorize Content Based on the Presence of 'Kill' and 'Violence' Keywords

```sql
WITH  new_table 
AS
(
SELECT *, 
       CASE 
	   WHEN 
	        description ILIKE '%kill%' OR
			description ILIKE '%violence%' THEN 'Bad_Content'
	   ELSE 'Good_Content'
END category
FROM netflix
)

SELECT 
      category ,
	  COUNT(*) AS Total_Content
FROM new_table
GROUP BY 1 ;
```






















