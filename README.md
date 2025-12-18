# Netflix Movies and Tv Shows Data Analysis using SQL

![Netflix logo](https://github.com/sejaladle12/netflix_sql_project/blob/main/logo.png)

## 📌 Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## 🎯 Objective
- Understand content distribution between Movies and TV Shows
- Identify popular ratings, countries, and actors on Netflix
- Analyze year-wise and country-wise content growth, especially for India
- Discover longest movies, multi-season TV shows, and recent content trends
- Categorize content using keyword-based analysis (e.g., violence, romance)
- Strengthen SQL skills using real-world queries (CTEs, joins, string functions, window functions)

## 🗂️ Dataset Information
- Dataset Name: Netflix Movies and TV Shows
- Format: CSV
- Total Records: ~8,800
- Source: Kaggle
- Columns: show_id, type, title, director, cast, country, date_added, release_year, rating, duration, listed_in, description

## 🏗️ Database Schema
```sql
create table netflix
(
show_id varchar(10),
type varchar(10),
title varchar(150),	
director varchar(208),
casts varchar(1000),	
country	varchar(150),
date_added date,	
release_year int,
rating	varchar(10),
duration varchar(15),	
listed_in varchar(100),		
description varchar(250)
);
```

## 🛠️ Tools & Technologies
- Database: PostgreSQL / MySQL
- Query Language: SQL
- Tools: pgAdmin, GitHub
- Concepts Used:CTEs,Window Functions,String Functions,Aggregations,Date Functions

## 🔍 SQL Solutions
### 1.Count the Number of Movies vs TV Shows
```sql
SELECT type, COUNT(*) FROM netflix GROUP BY 1;
```

### 2.Find the Most Common Rating for Movies and TV Shows
```sql
WITH RatingCounts AS ( SELECT type,rating,COUNT(*) AS rating_count FROM netflix
GROUP BY type, rating ),
RankedRatings AS ( SELECT type,rating,rating_count,
RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank FROM RatingCounts )
SELECT type,rating AS most_frequent_rating FROM RankedRatings WHERE rank = 1;
```

### 3.List All Movies Released in a Specific Year (e.g., 2020)
```sql
SELECT * FROM netflix WHERE release_year = 2020;
```

### 4.Find the Top 5 Countries with the Most Content on Netflix
```sql
SELECT country,COUNT(*) AS total_content FROM netflix
WHERE country IS NOT NULL GROUP BY country ORDER BY total_content DESC LIMIT 5;
```

### 5.Identify the Longest Movie
```sql
SELECT title, duration, type FROM netflix
WHERE type = 'Movie' AND duration = '^[0-9]+ min$'
ORDER BY CAST(split_part(duration, ' ', 1) AS INTEGER) DESC LIMIT 5;
```

### 6.Find Content Added in the Last 5 Years
```sql
SELECT * FROM netflix
WHERE date_added >= CURRENT_DATE - INTERVAL '5 years';
```

### 7.Find All Movies/TV Shows by Director 'Rajiv Chilaka'
```sql
SELECT title,type,director FROM netflix
WHERE director LIKE '%Rajiv Chilaka%';
```

### 8.List All TV Shows with More Than 5 Seasons
```sql
SELECT * FROM netflix
WHERE type = 'TV Shows' AND SPLIT_PART(duration, ' ', 1)::INT > 5;
```

### 9.Find each year and the average numbers of content release in India on netflix.return top 5 year with highest avg content release!
```sql
SELECT country,release_year,COUNT(show_id) AS total_release,
ROUND (COUNT(show_id)::numeric / (SELECT COUNT(show_id) FROM netflix WHERE country = 'India')::numeric * 100, 2) AS avg_release
FROM netflix WHERE country = 'India'
GROUP BY country, release_year
ORDER BY avg_release DESC LIMIT 5;
```

### 10.List All Movies that are Documentaries
```sql
SELECT * FROM netflix
WHERE listed_in LIKE '%Documentaries';
```

### 11.Find All Content Without a Director
```sql
SELECT * FROM netflix
WHERE director IS NULL;
```

### 12.Find How Many Movies Actor 'Salman Khan' Appeared in the Last 10 Years
```sql
SELECT * FROM netflix 
WHERE type = 'Movie' AND casts LIKE '%Salman Khan%' AND date_added >= CURRENT_DATE - INTERVAL '10 years';
```

### 13.Find the Top 10 Actors Who Have Appeared in the Highest Number of Movies Produced in India
```sql
SELECT UNNEST(STRING_TO_ARRAY(casts, ',')) AS actor,COUNT(*) FROM netflix
WHERE country = 'India' GROUP BY actor 
ORDER BY COUNT(*) DESC LIMIT 10;
```

### 14.Categorize Content Based on the Presence of 'Kill' and 'Violence' Keywords
```sql
SELECT title,type,
CASE WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Violent Content'
ELSE 'Non-Violent Content'
END AS content_category
FROM netflix;
```

### 15.Categorize Content Based on the Presence of 'Love' and 'Romance' Keywords
```sql
SELECT title,type,
CASE WHEN description ILIKE '%Love%' OR description ILIKE '%Romance%' THEN 'Romantic Content'
ELSE 'Non-Romantic Content'
END AS content_category
FROM netflix;
```

## ✅ Conclusion
This project analyzes Netflix’s movies and TV shows dataset using SQL to uncover insights related to content distribution, ratings, countries, actors, and release trends. The analysis demonstrates strong SQL skills, including data filtering, aggregation, CTEs, and string manipulation, and provides meaningful, data-driven insights from a real-world dataset.

## 👩‍💻 Author
Sejal Adle
🔗 GitHub:https://github.com/sejaladle12







