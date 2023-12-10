# MUSIC-RECOMMENDATION-SYSTEM (Outdated Design, will update soon)

Getting the song meta-data using a playlist link (*Playlist Link* or *Song ID* would be used), cleaning the data, processing it, adding extra columns, normalising data with *recency factor* and deriving the vector needed to recommend the songs. Comparing the songs using this vector and calculating the recommendation score (or *cosine similarity score*). Sorting songs based on the score in descending order then displaying it on the website along with their thumbnails. If possible, songs would be played on the website itself.
Also, if song id is used then same procedures needs to be followed.

Songs to be recommended will be selected using a pre-downloaded song meta-data, from which we will analyse every 100-150 songs (or whatever the capacity of our website) at once and not the entire dataset to make the process faster. Song thumbnails will be displayed along with the name.
There would be a lot of songs in the dataset, so to keep the recommendation fresh, we would be shuffling the dataset every run.
Won't be using the genre dataset as it is not publicly available through the spotify api. Yet, the known meta-data is kinda sufficient to fetch us some great recommendations.

**Sentiment part:** Using Deepface and openCV, we would be detecting the emotion on the face. User photo would be captured, and based on the dominant emotion, we would be considering the range of valence for filtering the large song data and then perform the recommendation process.
*How are we going to decide the score range for the emotion?* Most probably by listing a few 100 songs and then rating them in 0-1 range would help us understand that part, though not sure if it really works.

**Recommendation vector:** Following operations would be performed on the user playlist dataframe. Two extra columns (Months behind or recency and weights) need to be added before this. Weights would be calculated using the recency. Then those weights would be multiplied with the respective rows and then columns would be added to get the recommendation playlist vector.
Weight factor would be used to calculate the weights (wf ** months) (wf = 1.09).
Also, note that the weights and months won't be summed up!

![image](https://user-images.githubusercontent.com/105958364/225664940-bb09d44a-03b8-499c-98a7-deab0fd10bb7.png)

**Recommendation algorithm:** Cosine similarity will be used to compare the songs from the data with the recommendation vector obtained using the playlist. From sklearn, cosine similarity would be imported and used to find the similarity between the song vector and recommendation vector.
This would then return a similarity score for each songs and then songs need to be arranged in desceding order based on the score.

**Implementation:** On website (Yet to be decided).


2.0: Try to use artists column to enchance the recommendtions.

To get the monthly recency: Subtract the year from curr_year. If o/p is 0, then just subtract the month from curr_month. Otherwise, curr_month + (12 - month) + (curr_year - year - 1) * 12

**For recency:** Now calculating the weights which are going to be multiplied with each of the rows individually.
For that we need to understand this that the weights must reduce the values of the older songs, so we need weights
to be between 0 and 1.
1 / recency is not working as it is drastically reducing the values whih can negatively impact the recommendations.
So, 0.9 ** recency might work. For recency 3, we get the weight as 0.729, which is totally fine as we tend to listen less to songs which are older than 3 months. Also, going below this value can trigger false recommendations
Through this we are actually reducing the effect of older (added) songs on our vector.


**About Updating the list of artists:** We can easily update the artists excel file by just running the extracting_artists.ipynb with respective playlist input. Then we have to run the extracting_data.ipynb just once with any playlist. Once all is done, we are good to go. We have to just then run the main file, and we get the latest recommendations.
