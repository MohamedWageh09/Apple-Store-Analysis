# Apple Store Analysis using SQL
Hello people!
Here i'll be providing the analysis process, insights and recommendiations for people that are planning to add a new App on Apple Store.
The Dataset contains 2 files, one for the App data and the second one is for App description and you can find it on Kaggle:
https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps?resource=download&select=AppleStore.csv
## Dashboard: https://www.novypro.com/project/apple-app-store-analysis-power-bi
  - EDA Analysis
  - Insights
  - Recommendations

 ## 1) EDA Analysis
 ### First let's check if the data has duplicates in both files
 ``` sql
SELECT COUNT(DISTINCT id) AS UniqueApps
FROM AppleStoreApps;
-----------------
SELECT COUNT(DISTINCT id) AS UniqueApps
FROM AppleStoreDescription;
```
Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/1b5f8bb6-fefb-4a49-b03d-98e3e7f48a91)

  - No duplicates, great :D 

### Taking a look at overall user ratings
``` sql
SELECT MIN(user_rating) AS Min, AVG(user_rating) AS AVG, MAX(user_rating) AS Max
FROM AppleStoreApps;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/991d7bf1-9648-4208-9fc7-b7bef6123d4f)

### Checking if there are missing values in both files:
``` sql
-- Checking Nulls in Apps
SELECT COUNT(*) AS MissingValues
FROM AppleStoreApps
WHERE track_name IS NULL OR user_rating IS NULL OR prime_genre IS NULL OR price IS NULL OR currency IS NULL;
-- Checking Nulls in Descriptions
SELECT COUNT(*) MissingValues
FROM AppleStoreDescription
WHERE track_name IS NULL OR app_desc IS NULL;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/74a8e7a0-f633-4b58-b9ef-0568ef2781b3)

### Taking a look at the prices on Apple Store and excluding free apps:
```sql
SELECT MIN(price) AS Min_Price, MAX(price) AS Max_Price
FROM AppleStoreApps
WHERE price != 0;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/89096385-f8e0-487d-b0f4-c3597b326e66)

### How many free and paid apps are there?
``` sql
SELECT SUM(CASE
	WHEN price > 0 THEN 1
	END) AS Paid_Apps,
	SUM(CASE
	WHEN price = 0 THEN 1
	END) AS Free_Apps
FROM AppleStoreApps;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/96a846f5-5bea-4725-bfa6-1bcf55362575)

### How many apps are there in each genre?
```sql
SELECT prime_genre,
	COUNT(id) AS NumApps
FROM AppleStoreApps
GROUP BY prime_genre
ORDER BY 2 DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/462e3755-e580-4ef6-8752-18ed04d61cb6)

- Games, Entertainment, Education and Photo & Video are leading! such a competitive genres ;)

### Minimum and Maximum umber of supprted languages is Apps:
```sql
SELECT MIN(lang#num) AS Min_Languages_Count,
	MAX(lang#num) AS Max_Languages_Count
FROM AppleStoreApps;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/ee6c8409-9c05-4160-8d88-4104a883d369)

- 75 Language app! wow that must be a languages teaching app :D

### Minimum and Maximum app description length:
```sql
SELECT MIN(LEN(app_desc)) AS Min_Desc_LEN,
	MAX(LEN(app_desc)) AS Max_Desc_LEN
FROM AppleStoreDescription;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/12b5650c-95cc-4ce1-8f2a-e52c0cda661d)

### Minimun and Maximum number of screenshots
```sql
SELECT MIN(ipadSc_urls#num) as Min_Screenshots, MAX(ipadSc_urls#num) as Max_Screenshots
FROM AppleStoreApps;
```
Ourput:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/c09ced0c-282a-49de-b193-ebf1b5315c83)


EDA ends here, Now it's time for the insights

# 2) Insights

## Users rating per app size
```sql
SELECT
    CASE WHEN size_bytes / 1000000 <= 1024 THEN '1 gb or less'
         ELSE 'more than 1 gb'
    END AS Size_Category,
    COUNT(id) AS AppsNum,
	AVG(user_rating)
FROM
    AppleStoreApps
GROUP BY
    CASE WHEN size_bytes / 1000000 <= 1024 THEN '1 gb or less'
         ELSE 'more than 1 gb'
    END
ORDER BY
    AppsNum DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/4d7f6ed3-9b11-4a6e-b8e1-29b7987389e1)

 - App sizes were stored in bytes, 1 mb = 1000000 byte
 - Higher rating for bigger apps, that means users doesn't care about app size

## High competitive genres:
```sql
SELECT TOP 10 prime_genre, AVG(user_rating) AS AVG_Rating
FROM AppleStoreApps
GROUP BY prime_genre
ORDER BY 2 DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/fda5c034-2ec3-4e81-b726-e0834075a70d)

 - I'll call those genres "the battlefield" as of the high competitivity there.

## Easy to enter genres (low competitive)
```sql
SELECT TOP 10 prime_genre, AVG(user_rating) AS AVG_Rating
FROM AppleStoreApps
GROUP BY prime_genre
ORDER BY 2;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/e0ba15b2-11e8-4b5d-bfba-90d09f08b77f)

- in those genres there is less satisfied means a good opportunity to enter with a good app


## Paid apps and Free apps average rating:
```sql
SELECT CASE
	WHEN price > 0 THEN 'Paid'
	ELSE 'Free'
	END AS App_Type,
	AVG(user_rating) AS AVG_Rating
FROM AppleStoreApps
GROUP BY CASE
	WHEN price > 0 THEN 'Paid'
	ELSE 'Free' END
ORDER BY AVG_Rating DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/f7d38573-8afb-4459-96a7-0a380338df0d)

## Checking if more languages supported apps have higher average rating or not:
``` sql
SELECT CASE
	WHEN lang#num < 10 THEN 'Less Than 10'
	WHEN lang#num BETWEEN 10 AND 30 THEN '10 to 30'
	ELSE 'More Than 30'
	END AS LanguagesSupported, AVG(user_rating) AS AVG_Rating
FROM AppleStoreApps
GROUP BY CASE
	WHEN lang#num < 10 THEN 'Less Than 10'
	WHEN lang#num BETWEEN 10 AND 30 THEN '10 to 30'
	ELSE 'More Than 30'
	END
ORDER BY AVG_Rating DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/cd26835e-72e6-446e-aae5-02bbaafda643)

## Does App description length affect the rating?
```sql
SELECT CASE
	WHEN LEN(b.app_desc) < 500 THEN 'Short'
	WHEN LEN(b.app_desc) BETWEEN 500 AND 1500 THEN 'Medium'
	ELSE 'Long'
	END AS Description_Length,
	AVG(a.user_rating) AS AVG_Rating
FROM AppleStoreApps a
JOIN AppleStoreDescription b 
	ON a.id = b.id
GROUP BY CASE
	WHEN LEN(b.app_desc) < 500 THEN 'Short'
	WHEN LEN(b.app_desc) BETWEEN 500 AND 1500 THEN 'Medium'
	ELSE 'Long'
	END
ORDER BY AVG_Rating DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/2ee85773-8b82-4cd7-9931-739c2be471b4)

- People prefer apps with a full good description

## Top rated app for each genre:
```sql
SELECT track_name,
	prime_genre,
	user_rating
FROM
	(SELECT track_name,
		prime_genre,
		user_rating,
		RANK() OVER(PARTITION BY prime_genre ORDER BY user_rating DESC, rating_count_tot DESC) as rank
	FROM AppleStoreApps) a
WHERE rank = 1;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/2207bbf8-5b2d-439d-9189-89cdb16fad89)

## Average price for each genre and number of times sold
```sql
SELECT prime_genre,
	AVG(price) as AVG_Price,
	COUNT(rating_count_tot) #_sold
FROM AppleStoreApps
WHERE price != 0
GROUP BY prime_genre
ORDER BY AVG_Price DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/e43ffb91-c0bc-4a27-bfae-9aed0f243915)


## How does top Apps in each genre look like?
```sql
SELECT track_name AS top_product, prime_genre, user_rating, price AS price_of_top_product, ROUND(avg_price, 2) AS avg_price_in_genre
FROM (
    SELECT
        track_name,
        prime_genre,
        user_rating,
        price,
        RANK() OVER (PARTITION BY prime_genre ORDER BY user_rating DESC, price DESC, rating_count_tot DESC) AS rank,
        AVG(price) OVER (PARTITION BY prime_genre) AS avg_price
    FROM AppleStoreApps
) a
WHERE rank = 1 AND price != 0;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/da0f1cf0-13fe-4c8e-b586-a7d493d4af7d)


## Does people like more screenshots?
```sql
SELECT CASE
	WHEN ipadSc_urls#num <= 3 THEN '1 to 3 screenshots'
	ELSE '3 to 5'
	END AS #_of_screenshots,
	AVG(user_rating) AS AVG_Rating
FROM AppleStoreApps
	GROUP BY CASE
		WHEN ipadSc_urls#num <= 3 THEN '1 to 3 screenshots'
		ELSE '3 to 5'
		END
	ORDER BY AVG_Rating DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/a8d4f23a-28e3-4a9e-acd9-ca755a2ef085)


# Recommendations:

1) Choose a low competitivity genre where people are unsatisfied with the current apps such as Catalogs, Finance, LifeStyle Or News.
2) Don't add many languages to the app as it doesn't affect the rating, put that effort in another part.
3) Take a look at the top rated app for each genre and see how can you provide something different / add a new idea.
4) Application description is VERY important and people likes apps that has a full description for it's features/services.
5) Application size isn't a big deal as long as you are providing a good app.
6) You can make paid app (if you provide a high quality and value app) then list a good price depending on the genre that i listed on the insights.
7) Provide 3 to 5 screenshots in the application description.











