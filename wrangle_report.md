 # <center ><font color='#458199'>Project: Wrangle & Analyze Data </font></center>


This report aims to elaborate on steps taken as part of data wrangling for this project. It is divided into three sections:

<ul>
    <li><a href="#gather">Gathering</a></li>
    <li><a href="#assess">Assessing</a></li>
    <li><a href="#clean">Cleaning </a></li>
</ul>


<a id='gather'></a>
## <font color='#458199'>Gathering </font>
Gathered data from 3 places:
-	**`'twitter-archive-enhanced.csv'` file is provided to us.**
    Read `'twitter-archive-enhanced.csv'` file that was provided through pandas read_csv function.

-	**`‘Image Predictions’` file has to be downloaded from given [url](https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv).**
    -	Used request.get(url) method to download the file from given url.
    -	The text was then written into 'image_predictions.tsv' file.
    -	Since `'image_predictions.tsv'` is a tab separated file, read it with sep=’\t’

-	**Get retweet and favourite count from twitter**
    -	Create api to read tweets from twitter using tweet_id
    -	Tweets were read using api.get_status method. 
    -	They were written into the file using json.dumps and read using json.loads


---

<a id='assess'></a>
## <font color='#458199'>Assessing </font>
Steps taken for assessing: 
-	I used df.info(), df.describe() methods to get a sense of datasets, null values in coulmns, their average or min/max value.
-	A lot of records have large numerators and denominator. Checked these records to check for their validity.
-	Dog stages have very less values filled in. Did value_counts() on the column to get a sense.
-	Name column also seems to have a lot of wrong values, like ‘a’, ‘an’, ‘the’, ‘None’ etc.
-	Verified the records where column ‘retweeted_status_id' is not null, are all retweets.
-	Records where ‘in_reply_to_status_id’ is not null, have wrong numerators and denominators.


Finally, I identified below mentioned issues in assessment.

### <font color='#458199'> Quality Issues </font>

**Twitter Archive Table - `archive_df`**
1.	'name' column has value 'None', 'a', 'an' and 'this'.
2.	'timestamp' is string datatype
3.	Records where 'retweeted_status_id', 'retweeted_status_user_id' and 'retweeted_status_timestamp' is not null, are retweets. drop these rows
4.	Afterwards, drop columns 'retweeted_status_id', 'retweeted_status_user_id' and 'retweeted_status_timestamp'
5.	Rows where 'in_reply_to_status_id' and 'in_reply_to_user_id' is not null, have wrong numnerator and denominator for ratings. These are not original tweets but replies actually.
6.	Afterwards, drop 'in_reply_to_status_id' and 'in_reply_to_user_id' columns
7.	tweet_id = 666287406224695296 actual rating is 9/10
8.	Rating parameters are not consistent. Add column with division result of Numerator and Denominator
9.	Tweets for 13 dogs have multiple stages filled in. Some of them are correct. Some will need to be changed.

**Image Predictions Table - `image_pred`**

10.	column 'img_num' is integer datatype - can be category

### <font color='#458199'>Tidiness Issues </font>
11.	archive_df - 'doggo', 'floofer', 'pupper', 'puppo' - value 'None' should be replaced by NaN and these columns can be merged into one
12.	archive_df and count_df should be merged as similar contents

---

<a id='clean'></a>
## <font color='#458199'>Cleaning </font>
Made a copy of the dataframe and took following steps.

### <font color='#458199'>Quality Issues </font>
**Twitter Archive Table - `archive_df`**
1.	Replace value by 'NaN' where the ‘Name’ column value is 'None', 'a', 'an' and 'this'.
2.	Change 'timestamp' datatype to datetime
3.	Drop records where 'retweeted_status_id', 'retweeted_status_user_id' and 'retweeted_status_timestamp' is not null.
4.	Drop columns 'retweeted_status_id', 'retweeted_status_user_id' and 'retweeted_status_timestamp'
5.	Drop rows where 'in_reply_to_status_id' and 'in_reply_to_user_id' is not null.
6.	Drop 'in_reply_to_status_id' and 'in_reply_to_user_id' columns
7.	Change rating to 9/10
8.	Add ‘Rating’ column with division result of Numerator and Denominator
9.	7 of 13 them are correct. 6 will be changed to correct values.

**Image Predictions Table - `image_pred`**

10.	Change datatype for column 'img_num' to category.

### <font color='#458199'>Tidiness Issues </font>
11.	archive_df - 'doggo', 'floofer', 'pupper', 'puppo' – Replace value 'None' by NaN and merge these columns into one.
12.	archive_df and count_df – Merge the tables.


