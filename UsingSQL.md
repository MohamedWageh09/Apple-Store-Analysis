# Apple Store Analysis using SQL
Hello people!
Here i'll be providing the analysis process, insights and recommendiations for people that are planning to add a new App on Apple Store.
The Dataset contains 2 files, one for the App data and the second one is for App description and you can find it on Kaggle:
https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps?resource=download&select=AppleStore.csv
  - EDA Analysis
  - Insights
  - Recommendations

 ## 1) EDA Analysis
 First let's check if the data has duplicates in both files
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

Taking a look at overall user ratings
``` sql
SELECT MIN(user_rating) AS Min, AVG(user_rating) AS AVG, MAX(user_rating) AS Max
FROM AppleStoreApps;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/991d7bf1-9648-4208-9fc7-b7bef6123d4f)

Checking is there are null missing values in both files:
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

Taking a look at the prices on Apple Store and excluding free apps:
```sql
SELECT MIN(price) AS Min_Price, MAX(price) AS Max_Price
FROM AppleStoreApps
WHERE price != 0;
```

Output:

![image](https://github.com/MohamedWageh09/Apple-Store-Analysis/assets/120044385/89096385-f8e0-487d-b0f4-c3597b326e66)

How many free and paid apps are there?
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



