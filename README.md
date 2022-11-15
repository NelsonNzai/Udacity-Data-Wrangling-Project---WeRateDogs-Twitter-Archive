# Udacity Data Wrangling Project - WeRateDogs Twitter Archive
The project presented is designed to perform data wrangling on the We Rate Dogs twitter account
data. The twitter account gives a funny evaluation of the dogs owned by other individuals. The
denominator of these scores is almost always 10. However, the numerators are frequently more
than 10 e.g. 11/10, 12/10, 13/10 as a personal testament to dogs being likeable the world over. I
cannot extend this compliment to cats on the other hand.
For the project we will work with 3 datasets: 
- The WeRateDogs Twitter archive 
- The tweet image predictions dataset 
- Additional data from the Twitter API
We will first clean all 3 datasets(data wrangling part), combine and store them in a csv file, then perform some visualizations.

## 1. Data Wrangling
The steps we will follow in the data wrangling process include:
- Gathering 
- Assessing 
- Cleaning

## Gathering
This step involves getting all three datasets mentioned above and reading them into our workspace, either a personal jupyter notebook file or the workspace provided by Udacity. To do this we will require to import important libraries. They include: pandas, os, requests, and tweepy.

### The WeRateDogs Twitter archive
This dataset has been gathered for us and only requires manual addition to the files on hand for our jupyter notebook. After manually adding it, we then read
the file into a workable dataframe through the pandas library.

### The tweet image predictions dataset
The images of dogs in every tweet from the WeRateDogs account have been passed through a neural network that classifies the breed of dogs to create the
image predictions dataset. To read this file into our jupyter notebook we use the requests library
to query the data through the url given and read it into a dataframe using the pandas library

### Additional data from the Twitter API 
Some key information is missing from the WeRateDogs
twitter archive that we currently have. The dataset does not contain any key metrics such as
the number of retweets or favourite count that might be useful should an analysis be needed.
Using Twitter’s Development portal, we will query this data from the WeRateDogs twitter account
through the twitter API. A useful library for this is tweepy. With the help of the Python library
Tweepy, you have very easy access to the Twitter API. We will gather the minimal quantity of
favorites and retweets. Using the tweet IDs from the WeRateDogs Twitter archive, we query the
1
Twitter API using Python’s Tweepy package to obtain the JSON information for each tweet. The
complete set of JSON data for each tweet is then saved in a file called tweet json.txt.
Each tweet’s JSON data should be written to its own line. In a pandas DataFrame that includes
retweet count and favorite count, read each line from the this.txt file into the pandas DataFrame.

## Assessing
At this stage of the wrangling process, we shall check our datasets for potential data quality and
data tidiness issues. We shall utilise both visual and programmatic assessment methods. Visual
assessment involves viewing the datasets and combing through to find any errors. Programmatic
assessments involves the use of functions to find issues. Some useful functions include: .head()
.sample(x) .info() .describe()
After performing the assessments the following data quality issues were found: 
1. The rating
numerator for index 2235 is 2 instead of 10 in the twitter-archive-enhanced file.
2. Incorrect data type for timestamp variable name in the twitter archive file. Incorrect data type for tweet_id in the twitter archive file. This would need to be changed to a category data type to enable better data wrangling. rating_numerator and rating_denominator should also be changed to a float datatype.
3. Missing variable values in the twitter archive dataset including:
- in_reply_to_status_id
- in_reply_to_user_id
- retweeted_status_id
- retweeted_status_user_id
- retweeted_status_timestamp
- expanded_urls
4. Missing variable values in the tweet-json dataset including:
- contributors
- coordinates
- extended_entities
- geo
- in_reply_to_screen_name
- in_reply_to_status_id
- in_reply_to_status_id_str
- in_reply_to_user_id
- in_reply_to_user_id_str
- place
- possibly_sensitive
- possibly_sensitive_appealable
- quoted_status
- quoted_status_id
- quoted_status_id_str
- retweeted_status
5. Getting rid of contributors, coordinates and geo columns in the tweet-json dataset as they
are empty.
6. p1, p2, and p3 in the Image predictions dataset do not have consistent letter format to give
the names of dogs i.e. some dog names have capitalised first letters while others do not.
7. There exist images in the image predictions dataset that are not dogs. We would need to get
rid of these as we are only interested in images of dogs.
8. We have some tweets that are not original ratings for the dogs. We need to get rid of retweets in our dataset.

Tidiness issues present in the datasets include: 
1. There is no need for 2 different
datasets(tweet-json file and the twitter archive file). The two can be merged.
2. floofer, puppo, and doggo columns that denote the stage in which the dog is in are mutually
exclusive except when a mistake occurs. They should therefore be in a single column.

## Cleaning
Cleaning involves solving the data quality and data tidiness issues that have been highlighted
during the assessing stage. This final part involves the define-code-test framework. This framework means first defining the problem, solving it, then testing your solution to see if it worked. Before this is done a copy of each dataset is made and renamed with the suffix '_clean'.
Data quality issues were solved as follows: 
1. Using .replace() to replace the 2 denominator with 10. 
2. Changing the datatype of timestamp to datetime through to_datetime. Using .astype to change the datatype for tweet_id to category, rating_numerator, rating_denominator  to float. 
3. Using drop.na to get rid of null values. 
4. Using drop.na to get rid of null values. 
5. Getting rid of these columns through pythons drop function.
6. Using .str.lower() to turn all values lowercase. 
7. Using the columns p1_dog, p2_dog and p3_dog that
states whether or not the predicted image is a dog, we shall drop all rows where this is false for all 3 columns.
8. We got rid of all rows where the columns retweeted_status_id, retweeted_status_user_id, and retweeted_status_timestamp is not empty.

Data tidiness issues were solved as follows: 
1. Using pandas’ merge function to merge the two datasets. This merged dataset was later combined with the image predictions dataset. 
2. We first replaced 'None' entries in the floofer, doggo, pupper, and puppo columns with blanks. We then combined the values in the 3 datasets into a mew column called 'stage'. Afterwards, we applied formatting to some entries in the stage columnn that had glued string names together.


## 2. Storing the data
Store the three combined dataset in a master csv file called twitter_archive_master.
## 3. Analysis and Visualizations
### Analytical Insights
In this part we set out by asking 3 questions:
1. What was the highest rating given?
2. Who received the most retweets?
3. What dog breed was submitted the most to We Rate Dogs twitter account for rating?

We answered the first question by getting the maximum rating numerator in the dataset. The highest rating ever given to a single dog was 1776. A quick twitter search reveals that it was given to Atticus, a dog sitting in front of the American flag. The reason for such a high rating then becomes clear(1776 was America's year of independence).
We got the most retweeted rating by getting the maximum retweet count. At a staggering retweet count of 79515, the 13/10 rated doggo who received this was quite adorably standing in a pool.
We got the most submitted dog breed by counting the number of dogs by breed through value_counts(). Golden retrievers were the most submitted dog breeds to the We Rate Dogs twitter account for rating.
### Visualization
To view the relationship between the favourite count and the retweet count, we created a scatter plot of the two. They are quite clearly positively related.
