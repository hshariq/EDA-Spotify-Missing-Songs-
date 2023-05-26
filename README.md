# EDA-Spotify-Missing-Songs-
Data Wrangling and EDA for Songs dataset of Spotify. The dataset included several measures for a song, along with their song names and URI on Spotify. There are three datasets provided, the "structured" is our initial dataset with a lot of missing values. Next we have "cleandata" dataset which has all the changes done through EDA. Laslty the artists file is an attempt to further enrich our dataset.


## Data Wrangling and Cleaning

An overview of our datas missing value:

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/missingmanomatrix.png" alt="images of missing" width="300" height="200" />
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/summissing.png" alt="sum" width="300" height="200" />
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/dendogram.png" alt="pattern" width="300" height="200" />
</div>

### Filling for Song Names:
We can see that for each missing song name, there is a corresponding URI. Hence we will be using the uri, to update missing songs, it requires client id and secret which can not be shared. This is simply using the Spotify API to go and get the song name for the corresponding URI, and filling it inthe song_name column.

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/songNamefill.png" alt="images of missingsongs" width="800" height="500" />
</div>

### Filling for Genre:
Genre has several unique values, hence we will have to look closely on methods to fill for it.

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/genreuniruqe.png" alt="images of missingsongs" width="300" height="300" />
</div>

We will be seeing if there is any significant association between 'genre' and the numeric columns: null hypothesis that there is no association between two categorical variables. Using chi-test we see:

![img for chi genre](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/genrechi.png)

The results show that there is no relationship between genre and other numeric columns since pvalue>0.05, no relation if we look at our sheet, there is a pattern, and between a number of occurnaces of same genre, there are some misisng values:
-> 9 missing in between 4567 of Dark trap,
-> 10 missing in between 1800+ of rap
Since these data are missing in simialr clusters, we will fill on assumption that same pattern is continuing. We have identified two clusters, we fill accoridngly:

![cluster image](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/clustergenre.png)

### Filling for Tempo:

Tempo has no correlations we use anova to see if it has any relation with non numeric columns, our null hypthesis is that there is is no significant relationship between the two variables.

![img tempo](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/checkingfortempo.png)

Since our p values are greater than 0.05, our null hypothesis is true. We look at tempos distubtion and see if we can apply any other method on it:

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/coodefortempodist.png" alt="codetempo" width="600" height="300" />
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/tempodistrubition.png" alt="tempodist" width="400" height="300" />
</div>

We can see to a great extnet tempo is normalised, and in a certian range, hence we fill it in using median

### Filling for Loudness:
We have analysed the datset and have dropped the extreme outlier, and now we can see that most of the data is concentrated at one peak. Hence now our Loudness is normialised, and median will be used to calcuate the misisng values and since very low missing vlaues, it would not be a problem.

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/loudness%20analysis.png" alt="image of loudness missing" width="600" height="300" />
</div>

#### Filling method:
![image of loudness filloing using impuation](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/fillingloudness.png)

### Filling for Danceability:
Higher danceability relates to high loudness This code finds the missing danceability values, splits the data into two sets (one with missing danceability values and one with complete data), fits a linear regression model on the complete data, and predicts the missing danceability values from the loudness values using the trained model.

![image of dance fill](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/Danceability.png)

### Analysis for Energy:
Since Energy and Speechiness is highly corelated we check if it is valuable info for us.

![Image of analysis enrgy](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/energy%20analysis.png)

We will use further univariate analysis to extract insights on how "energy" is behaving:

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/energyboxplot.png" alt="Box Plot" width="400" height="300" />
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/energyskweness.png" alt="Skewness" width="400" height="300" />
</div>


We can see that the right lower quadrant is majory filled, that shows that for upper half of energy, there is lower half of speechiness which suggests that higher enery level songs mostly have lower speechiness. this shows negative corelation. We will be filling energy using speechiness as predicotr in our regression model for that we will first fill speechiness

### Filling Speachiness:
To fill Speachiness we look at how it is corelated with a column that is majorly filled.

Lets check for instrumentalness:

![img of corelation](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/coreofInstxSpeech.png)

Our general understanding suggests that more instruments mean low speech, we check this with our data using scatter plot corelation matrix does not help us, so we use scatter plot:

<div style="display:flex;">
  <img src="https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/instXspeech.png" alt="scat Plot" width="400" height="300" />
</div>

Here we can see that as instumentalness increases, speachiness drops alot, hence we will be filling Speachiness by building a linear regression model against instrumentalness:

![filling img](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/fillspeach.png)

### Filling Energy:

![filling enerfgy img](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/fillingenergy.png)

## DATA ENRICHING: ADDING ARTIST NAMES USING SPOTIFY API

One of the steps of data wrangling is "Enhancement", that is adding data to ur dataset. So that is what we are doing here, we are adding the column "artist_name" by using the spotify api to get the name of songs artists, this will be using the song_name, to add artist name, it requires client id and secret which can not be shared.
This is simply using the Spotify API to go and get the song name for the corresponding song_name, and filling it inthe artist_name column:

![enriching image](https://github.com/hshariq/EDA-Spotify-Missing-Songs-/blob/main/image/getArtist.png)

