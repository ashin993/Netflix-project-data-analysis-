# Netflix-project-data-analysis-

# Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

# Objectives
Analyze the distribution of content types (movies vs TV shows).
Identify the most common ratings for movies and TV shows.
List and analyze content based on release years, countries, and durations.
Explore and categorize content based on specific criteria and keywords.
#Dataset
The data for this project is sourced from the Kaggle dataset:

# Dataset Link: Movies Dataset
# Schema

create table titles
(
show_id varchar(75),
type	varchar(75),
title varchar(75),
director varchar(1000),
casts varchar(750),
country	varchar(75),
date_added	varchar(75),
release_year int,
rating	varchar(75),
duration	varchar(75),
listed_in  varchar(75),
description  varchar(259)
)

alter table titles 
alter column director type varchar(1000);

alter table titles 
alter column listed_in type varchar(1000);

alter table titles 
alter column country type varchar(1000);

alter table titles 
alter column title type varchar(1000);

alter table titles 
alter column casts type varchar(1000);



select * from titles

# --  query to implemented for the projects --
# -- count the number of movies vs tv shows --

 select type,
 count(*) as total_content 
 from titles
 group by type

# -- find the most common rating for movies and tv shows --

select  type , 
max(rating)
from titles
group by 1

# -- list all the movies released in specific year(2020) --

select count(title)  from titles 
where release_year=2020

# -- find the 5 countries with the most watched content on netflix --

select country, 
unnest (string_to_array(country,',')) as new_country 
from titles
group by 1

# -- selec the lonegst movie --

select max(duration) from titles

# -- find the content in the last five years --

select
       *,
from titles
where 
to_date(date_added, 'month , dd, yyyy') >= current_date-interval '5 years'

# -- movies given by director "rajiv chilaka"  --
 
 select title from titles
 where
 director='Rajiv Chilaka'

 # -- list tv show with more than 5 seasons  --

 select *
 from titles
 where
 type = 'Tv Show'
 AND
 SPLIT_PART(duration,' ',1 )::numeric> 5 

#   -- number of content in each genre  --

 select
 unnest(string_to_array(listed_in, ',')),
count(show_id)
from titles
 group by 1

 # -- find the each year and content relaeased average by india --

 select 
 extract(year from to_date(date_added,'month , dd , yyyy' )),
 count(*)
 from titles
 where country='India'
 group by 1

 # -- list all movies which are documentaries--

select 
unnest(string_to_array(listed_in, ','))
from titles
where type='Movies'

# -- 10 actors who appereared in highest number of movies --

select 
unnest(string_to_array(casts, ',')),
count(*) as new_field
from titles
where country ='India'
group by 1
order by 2 desc
limit 10

# -- categorixe content based on kill and voilence and rename them  --

select 
*,
  case 
  when description ilike '%kill%'
  or
  description ilike '%voilence%' then 'bad film'
  else 'good content'
  end category
  from titles


# Findings and Conclusion
Content Distribution: The dataset contains a diverse range of movies and TV shows with varying ratings and genres.
Common Ratings: Insights into the most common ratings provide an understanding of the content's target audience.
Geographical Insights: The top countries and the average content releases by India highlight regional content distribution.
Content Categorization: Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.
This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.
