# Beer-brewery-SQL-analysis

Analysis of US beers and breweries to see trends by state, style, and region

This is a simple analysis made in Postgres with a dataset of beers and breweries found on Kaggle:
https://www.kaggle.com/nickhould/craft-cans

## Queries

### Display the number of offerings in descending order by state
SELECT br.state, count(b.name) AS "Number of Offerings"
FROM breweries AS br, beer AS b
WHERE b.brewery_id = br.brewery_id
GROUP BY br.state
ORDER BY "Number of Offerings" DESC;

<img width="622" alt="image" src="https://user-images.githubusercontent.com/72087263/188255184-48fe61a1-d5bb-48b7-8486-af801c59a93c.png">

### Display the number of breweries operating in each state and order by desc number
SELECT br.state, count(br.*) AS "Number of Breweries"
FROM breweries AS br
GROUP BY br.state
ORDER BY "Number of Breweries" DESC;

<img width="616" alt="image" src="https://user-images.githubusercontent.com/72087263/188255199-d1d85c46-cab3-46a0-9841-0a35fed8d7fc.png">

### Display the number of beers by style and provide the average style abv in desc order
SELECT b.style, COUNT(*) AS "Number of Beer Offerings", ROUND(AVG(b.abv),3) AS "Average Style ABV",
	ROUND(AVG(b.ibu)) AS "Average Style IBUs"
FROM beer as b, breweries AS br
WHERE b.brewery_id = br.brewery_id
GROUP BY b.style
ORDER BY "Average Style ABV" DESC;

<img width="624" alt="image" src="https://user-images.githubusercontent.com/72087263/188255221-a50bc73b-d589-4c8d-8ef7-188a519dcae3.png">

### Display the number of beers by style and and provid average style abv, list in desc order of offerings
SELECT b.style, COUNT(*) AS "Number of Beer Offerings", ROUND(AVG(b.abv),3) AS "Average Style ABV",
	ROUND(AVG(b.ibu)) AS "Average Style IBUs"
FROM beer as b, breweries AS br
WHERE b.brewery_id = br.brewery_id
GROUP BY b.style
ORDER BY "Number of Beer Offerings" DESC;

<img width="620" alt="image" src="https://user-images.githubusercontent.com/72087263/188255242-c7f6d7c6-b71f-4e44-9ee7-dc9ecb457203.png">

### #Display the number of beers by style and and provid average style abv, list in desc order of IBUs
SELECT b.style, COUNT(*) AS "Number of Beer Offerings", ROUND(AVG(b.abv),3) AS "Average Style ABV",
	ROUND(AVG(b.ibu)) AS "Average Style IBUs"
FROM beer as b, breweries AS br
WHERE b.brewery_id = br.brewery_id
GROUP BY b.style
ORDER BY "Average Style IBUs" DESC;

<img width="621" alt="image" src="https://user-images.githubusercontent.com/72087263/188255279-ee6f8f4c-b973-4bbf-aa7c-4a0a4ca342d5.png">

### Display the beer style, number of offerings, and average abv grouped by style. Omit IPAs, pilsners, and lagers and display only groups with >= 10% abv
SELECT b.style, COUNT(*) AS "Number of Beer Offerings", ROUND(AVG(b.abv),3) AS "Average Style ABV"
FROM beer as b, breweries AS br
WHERE b.brewery_id = br.brewery_id
AND b.id NOT IN
	(SELECT b.id
    FROM beer AS b
    WHERE b.style LIKE '%IPA'
	OR b.style LIKE '%Lager'
	OR b.style LIKE '%Pilsner'
	OR b.style LIKE '%India Pale Ale'
	OR b.style LIKE '%Pilsener')
GROUP BY b.style
HAVING COUNT(*) >= 10
ORDER BY "Number of Beer Offerings" DESC;

<img width="615" alt="image" src="https://user-images.githubusercontent.com/72087263/188255307-725f25d6-4f5f-470f-bbd7-89b875cb4e2b.png">

### Break the breweries down by region (West, Midwest, Southwest, Southeast, Northeast) and display the number of breweries in each region
SELECT *
FROM
	(
	SELECT COUNT(br.*) AS "West"
	FROM breweries AS br
	WHERE br.state LIKE '%AK%'
	OR br.state LIKE '%HI%'
	OR br.state LIKE '%WA%'
	OR br.state LIKE '%OR%'
	OR br.state LIKE '%CA%'
	OR br.state LIKE '%NV%'
	OR br.state LIKE '%ID%'
	OR br.state LIKE '%MT%'
	OR br.state LIKE '%WY%'
	OR br.state LIKE '%CO%'
	OR br.state LIKE '%UT%'
	) AS "West",
	(
	SELECT COUNT(br.*) AS "Southwest"
	FROM breweries AS br
	WHERE br.state LIKE '%AZ%'
	OR br.state LIKE '%NM%'
	OR br.state LIKE '%TX%'
	OR br.state LIKE '%OK%'
	) AS "Southwest",
	(
	SELECT COUNT(br.*) AS "Midwest"
	FROM breweries AS br
	WHERE br.state LIKE '%ND%'
	OR br.state LIKE '%SD%'
	OR br.state LIKE '%NE%'
	OR br.state LIKE '%KS%'
	OR br.state LIKE '%MN%'
	OR br.state LIKE '%IA%'
	OR br.state LIKE '%MO%'
	OR br.state LIKE '%WI%'
	OR br.state LIKE '%IL%'
	OR br.state LIKE '%MI%'
	OR br.state LIKE '%OH%'
	OR br.state LIKE '%IN%'
	) AS "Midwest",
	(
	SELECT COUNT(br.*) AS "Southeast"
	FROM breweries AS br
	WHERE br.state LIKE '%AR%'
	OR br.state LIKE '%LA%'
	OR br.state LIKE '%MS%'
	OR br.state LIKE '%KY%'
	OR br.state LIKE '%TN%'
	OR br.state LIKE '%AL%'
	OR br.state LIKE '%FL%'
	OR br.state LIKE '%GA%'
	OR br.state LIKE '%SC%'
	OR br.state LIKE '%NC%'
	OR br.state LIKE '%VA%'
	OR br.state LIKE '%WV%'
	OR br.state LIKE '%DC%'
	OR br.state LIKE '%MD%'
	OR br.state LIKE '%DE%'
	) AS "Southeast",
	(
	SELECT COUNT(br.*) AS "Northeast"
	FROM breweries AS br
	WHERE br.state LIKE '%PA%'
	OR br.state LIKE '%NJ%'
	OR br.state LIKE '%NY%'
	OR br.state LIKE '%CT%'
	OR br.state LIKE '%RI%'
	OR br.state LIKE '%MA%'
	OR br.state LIKE '%VT%'
	OR br.state LIKE '%NH%'
	OR br.state LIKE '%ME%'
	) AS "Northeast";

<img width="621" alt="image" src="https://user-images.githubusercontent.com/72087263/188255338-490b95d0-ac14-44f7-8d7d-a14d2058d1a2.png">

### Display the number of offerings by bottle size
SELECT b.ounces, COUNT(*) AS "Number Available"
FROM beer AS b
GROUP BY b.ounces;

<img width="507" alt="image" src="https://user-images.githubusercontent.com/72087263/188255383-cd72072c-a718-43ed-af44-a42aa114611f.png">

### Display the name, number of offerings, and average abv for each brewery and list in desc order of offerings
SELECT br.name, COUNT(b.id) AS "Number of Offerings", ROUND(AVG(b.abv),3) AS "Average ABV"
FROM breweries AS br, beer AS b
WHERE br.brewery_id = b.brewery_id
GROUP BY br.name
ORDER BY "Number of Offerings" DESC;

<img width="604" alt="image" src="https://user-images.githubusercontent.com/72087263/188255415-727bf228-7558-40ec-a71d-9e40182b718f.png">

