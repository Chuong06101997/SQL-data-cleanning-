# SQL-data-cleanning-
This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

## Data Introduction
```sql
SELECT * FROM club_member_info cmi
```
The results:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

## Copy Table
### Create new table for cleaning
```sql
-- club_member_info definition

CREATE TABLE club_member_info_cleanned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
### Copy all value from original table
```sql
INSERT INTO club_member_info_cleanned 
SELECT * FROM club_member_info;
```
### Cleaned full_name column
```sql
UPDATE club_member_info_cleanned cmic ed SET full_name = TRIM(full_name);
UPDATE club_member_info_cleanned cmic ed SET full_name = UPPER(full_name);
UPDATE club_member_info_clean SET full_name = REPLACE(REPLACE(full_name, '!', ''), '?', '')
WHERE full_name LIKE '%!%' OR full_name LIKE '%?%';
```
The results:
|full_name|
|---------|
|ADDIE LUSH|
|ROCK CRADICK|
|SYDEL SHARVELL|
|CONSTANTIN DE LA CRUZ|
|GAYLOR REDHOLE|
|WANDA DEL MAR|
|JOANN KENEALY|
|JOETE CUDIFF|

### Clean Age colummn
```sql
UPDATE club_member_info_cleanned
SET age =
	CASE
		WHEN LENGTH(age) > 2
		THEN SUBSTR(age, 1, LENGTH(age) - 1)
		ELSE age 
	END;
```
The results:
|age|
|---|
|40|
|46|
|35|
|38|
|44|
|41|
|51|
|52|
|42|
|29|

### Cleaned martial_status column
```SQL
UPDATE club_member_info_cleanned
SET martial_status = 'Unknown'
WHERE martial_status = '';
UPDATE club_member_info_cleanned
SET martial_status = 'divorced'
WHERE martial_status = 'divored';
```
The results:

|martial_status|
|--------------|
|married|
|married|
|divorced|
|Unknown|
|married|
|single|
|married|
|divorced|
|single|
|married|

### Cleaned phone column
```sql
UPDATE club_member_info_cleanned SET phone = REPLACE(phone, '-', ' ');
UPDATE club_member_info_cleaned SET phone = 'Unknown'
WHERE phone  = '';
```
The Results:
|phone|
|-----|
|254 389 8708|
|910 566 2007|
|702 187 8715|
|402 688 7162|
|917 394 6001|
|937 467 6942|
|513 726 9885|
|616 617 0965|
|504 918 4753|
|808 177 0318|
|203 993 0118|
|805 968 3034|
|612 914 2658|
|702 364 0009|
|608 659 4566|

### Cleaned job_title column
```sql
UPDATE club_member_info_cleanned
SET job_title = 'Unknown'
WHERE job_title   = '';
```

|job_title|
|---------|
|Assistant Professor|
|Programmer III|
|Budget/Accounting Analyst I|
|Desktop Support Technician|
|Legal Assistant|
|Human Resources Assistant IV|
|Accountant IV|
|Research Nurse|
|Systems Administrator III|
|Chemical Engineer|
|Chemical Engineer|
|Programmer I|
|Business Systems Development Analyst|


### full_address
```sql
UPDATE club_member_info_cleaned 
SET full_address = 'UnKnown'
WHERE full_address LIKE '0%';
```
The results:
|full_address|
|------------|
|3226 Eastlawn Pass,Temple,Texas|
|4 Harbort Avenue,Fayetteville,North Carolina|
|4 School Place,Las Vegas,Nevada|
|6 Monument Crossing,Omaha,Nebraska|
|88 Cherokee Pass,New York City,New York|
|10864 Buhler Plaza,Hamilton,Ohio|
|733 Hagan Parkway,Cincinnati,Ohio|
|975 Dwight Plaza,Grand Rapids,Michigan|
|34 Delladonna Terrace,New Orleans,Louisiana|
|8976 Jackson Park,Honolulu,Hawaii|
|2254 Express Hill,New Haven,Connecticut|
|UnKnown|
|61 Blue Bill Park Plaza,Minneapolis,Minnesota|


### Cleaned membership_date
```sql
UPDATE club_member_info_cleanned
SET membership_date = REPLACE(membership_date, '1,9', '2,0')
WHERE membership_date LIKE '1,9%';

UPDATE club_member_info_cleanned
SET membership_date = REPLACE(membership_date, '/19', '/20')
WHERE membership_date LIKE '%/19__'; 
```
the results:
|membership_date|
|---------------|
|7/31/2013|
|5/27/2018|
|10/6/2017|
|10/20/2015|
|5/29/2019|
|3/24/2015|
|4/17/2013|
|11/16/2014|
|3/12/2021|
|11/5/2014|

# Remove Duplicates - Lecture 8: Subqueries. SQL quality-of-life rules
## Retrieve duplicates
```sql
SELECT ROWID, * FROM club_member_info_cleanned
WHERE email IN (
	SELECT email FROM club_member_info_cleanned
	GROUP BY email 
	HAVING COUNT(*) > 1
)
ORDER BY email;
```
=> In the first 20 rows, there are 15 duplicate rows, and only 5 unique (non-duplicate) rows.

|rowid|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|-----|---------|---|--------------|-----|-----|------------|---------|---------------|
|221|ARDA ALLAM|36|married|aallam64@nps.gov|415 797 9281|86 Sunfield Parkway,San Rafael,California|Human Resources Assistant I|1/10/2012|
|2231|ARDA ALLAM|36|married|aallam64@nps.gov|415 797 9281|86 Sunfield Parkway,San Rafael,California|Human Resources Assistant I|1/10/2012|
|4241|ARDA ALLAM|36|married|aallam64@nps.gov|415 797 9281|86 Sunfield Parkway,San Rafael,California|Human Resources Assistant I|1/10/2012|
|6251|ARDA ALLAM|36|married|aallam64@nps.gov|415 797 9281|86 Sunfield Parkway,San Rafael,California|Human Resources Assistant I|1/10/2012|
|789|AERIELL ANGELINI|32|married|aangelinilu@whitehouse.gov|806 508 7374|UnKnown|Database Administrator I|7/10/2013|
|2799|AERIELL ANGELINI|32|married|aangelinilu@whitehouse.gov|806 508 7374|UnKnown|Database Administrator I|7/10/2013|
|4809|AERIELL ANGELINI|32|married|aangelinilu@whitehouse.gov|806 508 7374|UnKnown|Database Administrator I|7/10/2013|
|6819|AERIELL ANGELINI|32|married|aangelinilu@whitehouse.gov|806 508 7374|UnKnown|Database Administrator I|7/10/2013|
|199|AURORE AVERILL|28|married|aaverill5i@theglobeandmail.com|713 330 3502|42107 Debs Court,Houston,Texas|Account Coordinator|5/11/2019|
|2209|AURORE AVERILL|28|married|aaverill5i@theglobeandmail.com|713 330 3502|42107 Debs Court,Houston,Texas|Account Coordinator|5/11/2019|
|4219|AURORE AVERILL|28|married|aaverill5i@theglobeandmail.com|713 330 3502|42107 Debs Court,Houston,Texas|Account Coordinator|5/11/2019|
|6229|AURORE AVERILL|28|married|aaverill5i@theglobeandmail.com|713 330 3502|42107 Debs Court,Houston,Texas|Account Coordinator|5/11/2019|
|866|ANNEMARIE BALSOM|52|divorced|abalsomny@wired.com|718 247 6744|723 Caliangt Park,Jamaica,New York|Design Engineer|3/6/2014|
|2876|ANNEMARIE BALSOM|52|divorced|abalsomny@wired.com|718 247 6744|723 Caliangt Park,Jamaica,New York|Design Engineer|3/6/2014|
|4886|ANNEMARIE BALSOM|52|divorced|abalsomny@wired.com|718 247 6744|723 Caliangt Park,Jamaica,New York|Design Engineer|3/6/2014|
|6896|ANNEMARIE BALSOM|52|divorced|abalsomny@wired.com|718 247 6744|723 Caliangt Park,Jamaica,New York|Design Engineer|3/6/2014|
|1923|ALICEA BAMELL|52|married|abamellpc@squidoo.com|850 167 7028|UnKnown|Help Desk Technician|9/14/2021|
|3933|ALICEA BAMELL|52|married|abamellpc@squidoo.com|850 167 7028|UnKnown|Help Desk Technician|9/14/2021|
|5943|ALICEA BAMELL|52|married|abamellpc@squidoo.com|850 167 7028|UnKnown|Help Desk Technician|9/14/2021|
|7953|ALICEA BAMELL|52|married|abamellpc@squidoo.com|850 167 7028|UnKnown|Help Desk Technician|9/14/2021|

