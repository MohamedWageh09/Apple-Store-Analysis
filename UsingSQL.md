# Apple Store Analysis using SQL
Hello people!
Here i'll be providing the analysis process, insights and recommendiations for people that are planning to add a new App on Apple Store.
The Dataset contains 2 files, one for the App data and the second one is for App description and you can find it on Kaggle:
https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps?resource=download&select=AppleStore.csv
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

### Checking is there are null missing values in both files:
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

### How many app is there in each genre?
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

## Average rating for each app genre:
```sql
SELECT TOP 10 prime_genre, AVG(user_rating) AS AVG_Rating
FROM AppleStoreApps
GROUP BY prime_genre
ORDER BY 2 DESC;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/fda5c034-2ec3-4e81-b726-e0834075a70d)

 - I'll call those genres "the battlefield" as of the high competitive there. 










