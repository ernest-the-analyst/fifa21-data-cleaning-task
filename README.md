# fifa21-data-cleaning-
![image](https://user-images.githubusercontent.com/128243939/228201137-ab395cbf-3dbe-4623-8768-60dbaf83eb7f.png)



# Introduction

This outlines how to clean the FIFA '21 dataset provided for a #Datacleaningchallenge on Twitter. The dataset has 18,979 rows and 77 columns of football player demographics and statistics from 2021. It's publicly available on [kaggle](https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring) and includes a Data dictionary and a "player url" column linking to player profiles on Sofifa for further details. I recommend using Power Query in Power BI for this challenge based on proficiency.

# Data Cleaning Process

![image](https://user-images.githubusercontent.com/128243939/228203373-6939b5b0-e8e5-4a8e-8d74-8438c8e4cafc.png)

After loading the data, the approach to ensuring data quality involved removing whitespaces by deselecting a button from the view tab.

<h1 align="center">Whitespaces</h1>

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224610838-f6b78c4b-b204-42d9-b158-f7aeacb6cda8.png) | ![image](https://user-images.githubusercontent.com/99989624/224610689-763b528d-9eed-431a-8b7d-2d5a54abaa32.png)


**1. Data Auditing** : To ensure data quality, certain columns were flagged for cleaning during the auditing stage, including Name, Longname, Age, OVA, POT, Club, Contract, Positions, Height, Weight, Best position, Joined, Loan End date, Value, wage, release clause, W/F, SM, IR, and Hits.


**2. Data enrichment** :In this step, extra information is added to the dataset to increase its value and usefulness. Since the data was collected in 2021, but is now two years old, an additional column was created to adjust the players' ages to their current values. The analyst or visualizer can choose to either use or drop the original age column.

<h1 align="center">Age </h1>

The age column + 2 gives us their age in 2023

| Before | After |
|--------| ------|
|![image](https://user-images.githubusercontent.com/99989624/224671691-bc17297f-d433-42b2-801b-2a5c614b0fff.png)|![image](https://user-images.githubusercontent.com/99989624/224671895-e15292ce-058d-4fa1-a691-7a36ed032e3d.png)|

**3. Data Cleaning and Transformation**

he columns that were marked earlier were addressed in a systematic manner.

<h1 align="center">Names </h1>

To standardize the Name and Longname columns, the data was split using delimiters and empty rows were replaced to create separate first name and last name columns. Special cases like hyphenated or double last names such as De Bruyne, Ter Stegen, and Van Dijk etc were handled appropriately.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224680924-00835e64-6273-4c6e-aab9-a472fe3cea3b.png)|![image](https://user-images.githubusercontent.com/99989624/224681183-81a31f33-425e-4105-a848-ebf7d3134ecc.png)

For diacritic names such as Spanish names with accents, the player URL was used to extract the names and obtain a cleaner version. To identify places with diacritics in the first letter of the alphabet, a filter was applied, and these were replaced with the clean version. Without this step, sorting the names in alphabetical order during visualization would result in these names appearing last after Z.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224684006-6287fa7c-6d31-408c-99bd-95125ad152d5.png)|![image](https://user-images.githubusercontent.com/99989624/224684358-ea94ad53-b786-43db-8d29-38e647689471.png)



To achieve standardization, names such as C. Ronaldo, A. Benjamin, and G. Paiva were modified to display the full first name

<div align="center">
    <h2>Percentages</h2>
</div>

Following the guidance of the Data Dictionary, the OVA and POT columns were reformatted to display percentages. To achieve this, the "Column from example" was utilized to add a "%" symbol to the end of the row figures, and the data type was changed to percentage.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224689449-f6a490f1-e016-4058-9373-71487095283d.png)|![image](https://user-images.githubusercontent.com/99989624/224689726-1884d63f-e22e-4b70-8a67-3dbead6aa4ed.png)

<div align="center">
    <h2>Clubs</h2>
</div>

Certain club names had "1." at the beginning, such as 1.FC Koln and 1.FC Union Berlin, and these were standardized. Additionally, certain club names had diacritic first letters, which, as previously mentioned, could cause alphabetical sorting to fail during visualization.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224834902-356b3ed5-c7e0-489c-87d2-b3602bc91364.png)|![image](https://user-images.githubusercontent.com/99989624/224835627-e81bbc9a-7c95-41e9-a1e3-5be5cef1ac2c.png)


<div align="center">
    <h2>Contract</h2>
</div>

The column in question had inconsistent data type and format, and to standardize it, a combination of three columns was used: Contracts, Joined, and Loan End Date. By using the filter view, players whose contract column indicated that they were on loan were cross-checked with the year on the Loan End Date column. Likewise, players whose contract indicated free transfer were matched with clubless players who had no wage, value, or release clause, and these were replaced with null since there was no specified loan end date, and they were not on contract. These players should be dropped during visualization.

After completing the cleaning, splitting, and merging of columns, we ended up with two columns: Contract Start and Contract End, which only display the year information due to a lack of more detailed data.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224838470-25cd2741-d7bf-479b-8ff7-14d2917f498c.png)|![image](https://user-images.githubusercontent.com/99989624/224838770-df41bf1a-5391-4bdd-8a17-99c4a7da1cb5.png)

The Contract Start and Contract End columns have a data type of ABC text, as formatting them as date columns would result in incorrect months and days being assigned to all rows (i.e., the first day of the first month would be automatically assigned). Therefore, during visualization, if these columns are formatted as dates, only the year should be used for drill down purposes.

<div align="center">
    <h2>Positions</h2>
</div>
 
 **Split ? Or ignore:** 
 The Position column contains multiple positions that some players have played in, and separating them into different columns would create too much irrelevant data, taking up unnecessary memory space. Instead, the Best position column contains complete information suitable for analysis, so the decision was made to drop the Position column.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224841755-aac24509-7c26-40e5-b46b-4cdc8ddca9e7.png)|![image](https://user-images.githubusercontent.com/99989624/224841978-f61c750b-4ae4-40fe-9af7-8f7208547b2b.png)


<div align="center">
    <h2>Height</h2>
</div>

The height column contains values in centimeters and also in feet and inches such as 6'2". Initially, there was a plan to convert all values to centimeters by multiplying the feet value by 30.48. However, it was discovered that this formula would not account for the inches value. To address this, the feet column was multiplied by 30.48 and the inches column by 2.54. Then, the two columns were added and rounded up to the nearest whole number. Finally, the "cm" text value was dropped, resulting in a successful conversion of the data type to numeric.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224844971-31f4648e-2b1f-42d8-99b7-84e1a376a428.png)| ![image](https://user-images.githubusercontent.com/99989624/224845368-72935580-ede4-44b1-94bd-d5db1d4866e8.png)


<div align="center">
    <h2>Weight</h2>
</div>
The weight column had around 60% of data values in pounds and 40% in kilograms. To unify the data, the column was split using the Digits to non-digits function. It was decided to convert all values to pounds. For the values in kilograms, the standard conversion rate of 2.205 was used to multiply the weight, and the result was rounded up to the nearest whole number.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224845690-2cb91c6f-bcf9-478f-b1d7-0dc5cc0e8b76.png)|![image](https://user-images.githubusercontent.com/99989624/224846437-d180ffa8-277e-400c-aa69-a2051aed01f5.png)


<div align="center">
    <h2>Value , Wage and release clause</h2>
</div>

Euro values in three columns are shortened, with 1.5k and 1.5m representing 1,500 euros and 1.5 million euros, respectively. The objective is to standardize the columns and convert them to US dollars. To achieve this, the euro symbol was removed from all rows, and the columns were split by "k" and "m." A new custom column was created, which multiplies the figure by 1,000,000 if column 1 contains "m," multiplies by 1000 if column 1 contains "k," else returns the figure on the original column. This approach gives the full numeric form of the value, wage, or release clause, which was then converted to USD using the average euro to USD exchange rate of 1.183, the average exchange rate as of 2021.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224848874-1db3b234-8f91-4a58-8aa8-62e4db072d38.png)|![image](https://user-images.githubusercontent.com/99989624/224849112-0e74cdb0-ceb6-4a38-bc4d-4efc3f9ccf43.png)

<div align="center">
    <h2>W/F , SM , IR</h2>
</div>
The columns contain player ratings with values ranging from 1-5, but each row had a star symbol encoded which was removed using the replace function. The data type was then changed to numeric.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224855858-546c4bdf-adb2-421a-a459-f799a7f1a241.png)|![image](https://user-images.githubusercontent.com/99989624/224855981-166a7132-bbb1-4451-89e5-62f9b43d66dd.png)

<div align="center">
    <h2>Hits</h2>
</div>

The column contained figures in a shortened format, such as 1.5k for 1500. To standardize the data, the same approach used for the values and wages column was applied.

| Before | After |
|--------|-------|
![image](https://user-images.githubusercontent.com/99989624/224856470-037630d2-1429-4644-b107-d5b88323fa32.png)|![image](https://user-images.githubusercontent.com/99989624/224856581-0ff90894-e6da-4edc-b25a-850a3b59e966.png)


<div align="center">
    <h2>CONCLUSION</h2>
</div>

After the challenging task, data validation was performed to ensure accuracy, completeness, and consistency. Participating in this #DatacleaningChallenge was a great learning experience for me as an intermediate analyst.

I am constantly seeking to improve my analytical skills and welcome any suggestions or recommendations. Additionally, I am happy to assist and guide other participants in this project.


You can reach out to me on Twitter | [@Moonligh17BC](https://twitter.com/Moonlight17BC) | email: [digitaladspaces@gmail.com] | [ernestizu@gmail.com]





