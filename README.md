# fifa21-data-cleaning-task
![image](https://user-images.githubusercontent.com/97708113/228183262-7c5f5ba0-89c8-4a61-8e1b-4eb052afcfd5.png)

# Introduction

This is a rundown of the cleaning process for FIFA '21 dataset. This data set was provided as part of a challenge #Datacleaningchallenge launched on twitter by data ethusiasts to test the prowess of newbies , intermediate and pro Data analyst alike for a large messy dataset.

The data set contains 18,979 rows and 77 columns of football players statistics and demography in 2021 The dataset is publicly available on [kaggle](https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring) which contains data scrapped from sofifa The datafile also includes a Data dictionary as well as a column named player url which contains a link to the player profile on [sofifa](https://github.com/Atohor/fifa-21-data-cleaning-task/blob/main/www.sofifa.com) to give the analyst furher insight and full details of the player in view in case of missing information.

My prefered tool for this Data cleaning challenge based on proficiency is Power Query as available on Power BI

## Data Cleaning Process

![image](https://user-images.githubusercontent.com/97708113/228185857-9b664ac7-436b-4060-9713-21a7c206703f.png)

To ensure data quality , the following approach was used after loading the data , whitespaces were removed by unticking the button from the viewtab

## Whitespaces
![image](https://user-images.githubusercontent.com/97708113/228186251-ba1d51e7-178d-4ea0-acf2-7fcfd1324200.png)


1. Data Auditing To ensure data quality, during the auditing stage of understanding the data and identifying any inconsistencies, missing values, or errors that need to be addressed, the following columns were marked for cleaning ; Name, Longname , Age , OVA , POT , Club , Contract , Positions , Height , Weight , Best position , Joined , Loan End date , Value, wage , release clause , W/F , SM , IR , and Hits.

2. Data enrichment This step involves adding additional information to the data,to enhance its value and usefulness. The dataset was collected 2 years ago in 2021 thus the data was modified to reflect their current age. This was made as an additional column to give the Analyst/Visualizer a choice to use or drop either one.

Age
the age column + 2 gives us their age in 2023

Before	After
image	image
3. Data Cleaning and Transformation

The previously identified columns were worked on , in an orderly manner

Names
The Name and Longname column were Standardised by splitting delimiters and replacing empty rows to generate a first name and last name column. Hyphenated or double last names like De Bruyne , Ter stegan , Van Dijk were also kept in the right manner

Before	After
image	image
However to sort Diacritic names, like spanish names with accent , The player url was used to extract those names to get a cleaner version. Filter to see places with Diacritics for the first letter of the alphabet and replace with clean version. If this is not done, during visualization, sorting the names in alphabetical order, ascending or descending will result in those names appearing last after Z.

Before	After
image	image
finally Names like C. Ronaldo , A. Benjamin , G. Paiva were standardized to reflect the full first name

Percentages
As advised by the Data Dictionary , the columns OVA and POT were reformatted to reflect percentages. Column from example was used to add % to the end of the row figures ,then the data type was changed to percentage

Before	After
image	image
Clubs
Some club names started with 1. examples 1.fc koln , 1. fc union Berlin these were normalised . Also some other club names contained diacritic first letters and as previously explained, this makes alphabetical sorting to flop during visualiization
Before	After
image	image
Contract
This column contained inconsistent data type and format, hence this was reguarized using a combination of 3 columns , namely; Contracts , Joined , and Loan end date The filter view reveals players whose contract column specify they are on loan tallies with the year on the Loan end date column. while players whose contract indicate free transfer, tally with those clubless players with no wage or value or release clause . this were replaced with null as there is no specified loan end date since they're not on contract and have no recorded wages. These players should therefore be dropped during visualization.

At the end of the cleaning, splitting and merging of columns we arrived at two columns : contract start and contract end which displays only the year info as lack of more data prevented further date drill down

Before	After
image	image
Notice the data type of the contract start and contract end data type is ABC text because formating it as a date column will lead to incorrect months and day values. As the first day of the first month is automatically assigned for all rows. Thus During visualization, when formatted as date , only the year drill down should be used.

Positions
Split ? Or ignore: This column contains the position the player has ever played and some had 2 or more positions assigned to them using comma seperated values. Initial thought was to split the column by delimiter to reflect all positions however since this will lead to creating too many irrelevant data occupying memory space,a decision was reached to drop the column for the best position column as best position column contains complete information suitable for analysis

Before	After
image	image
Height
The majority of values in this column displays height in centimeter. while a few others uses feet and inches for example 6'2" . Initial thought was just to split by the delimiter ' " and multiply by 30.48 which is the standard conversion of feet to cm . However a quick google search reveals this formula only accounts for feet and not inches hence to resolve this after splitting, the feet column was multplied by 30.48 and the inches column by 2.54. then both columns were merged using addition and rounded up to the nearest whole number. then the cm text value was dropped from the other rows to successfully change the data type to numeric

Before	After
image	image
Weight
About 60% of the data values in this column contains weight in lbs (pounds) and the other 40% in Kg . After splitting the column using Digits to non-digits function, A decision was made to unify all data values as lbs and the standard conversion rate of 2.205 was used to multiply the weight in Kg value to give the equivalent in lbs and rounded up.
Before	After
image	image
Value , Wage and release clause
These three columns contains euro values in shortenend format where 1,500 euros was written as 1.5k and one million ,five hundred euros as 1.5m. The goal is to standardise the columns and convert to US dollars Hence the approach used was to dropped the euro symbol from all rows using find and replace, then split columns by m and k
first step was to create a New custom column which If column 1 contains m multiplies the figure by 1,000,000 then if column 1 contains k multiply by 1000 else return the figure on the original column , this else function handles the values , wages or release clause in hundreds having no K or M attached (some players earn as low as 500 euro wage ).

this approach gives the full numeric form of the value , wage or release clause but wait! we are not done yet . To convert this full numeric digits to USD , the average euro to USD exchange rate of 1.183 was used as this was the average exchange rate as at 2021 which our data is based on.

Before	After
image	image
W/F , SM , IR
These three columns contains player ratings in different aspect with a ranking of 1-5 . However a star symbol was encoded to each row and this was removed using the replace function and the data type was changed to numeric.
Before	After
image	image
Hits
The filter pane reveals some figures in this column were coded in the shortened version, such as 1500 written as 1.5k. These were regularized using a similar approach as previously used and explained for the values and wages column.
Before	After
image	image
CONCLUSION
At the end of the day, Data validation was done to verify that the data is accurate, complete, and consistent. Conclusively after what posed to be a Herculean task at first glance, i'm glad i rose up to the occassion to take the bull by the horn by participating in this #DatacleaningChallenge. This has served as a great learning curve for an Intermediate level analyst like me.
Honing my analytical skills is a never ending journey hence i'm open to suggestions , recommendations and improvement . I'm also willing to guide other participants in this project.
