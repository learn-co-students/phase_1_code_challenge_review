
# Phase 1 Code Challenge Review

![let's do this](https://media.giphy.com/media/BpGWitbFZflfSUYuZ9/giphy.gif)

The topics covered will be:

  - [Interacting with Pandas dataframes](#dataframes)
  - [Visualization](#viz)
  - [Python Data Structures](#datastructures)
    

<a id='dataframes'></a>
# DataFrames

To practice working with dataframes, we will use some Facebook data taken from the UCI Machine Learning repository.

Refer to this paper if you are interested in learning more. There is also a nice description of the features: http://www.math-evry.cnrs.fr/_media/members/aguilloux/enseignements/m1mint/moro2016.pdf



# Task 1

Read 'dataset_Facebook.csv' from data/Facebook_metrics into the notebook as a Pandas dataframe.


```python
facebook = pd.read_csv('data/Facebook_metrics/dataset_Facebook.csv',delimiter=';')
```

# Task 2

### 2a: Count how many na's there are in each column
 


```python
facebook.isna().sum()
```

### 2b: Drop records that have na's in any column without altering the dataframe in memory
 


```python
facebook.dropna()
```

### 2c: Drop records that have na's in the `share` column while altering the dataframe in memory


```python
facebook.dropna(subset=['share'], inplace=True)
```

# Task 3

An impression is each time a post is displayed.  

Create a new column called `likes_per_impression` which divides the number of comments per post by the number of likes per post.


```python
facebook['likes_per_impression'] = facebook['like']/facebook['Lifetime Post Total Impressions']
```

# Task 4

Locate the `record` of a **Photo** that has the largest value in the `like` column


```python
facebook[(facebook['Type']=='Photo') & (facebook['like']==facebook['like'].max())]
```

# Task 5
What is the mean number of Total Interactions for photos?


```python
facebook[facebook['Type'] == 'Photo']['Total Interactions'].mean()
```

<a id='viz'></a>
# Visualization

# Task 6

Create a bar chart showing the number of posts per month.
Order the x-axis by month as they appear on the calendar.
Don't forget to add labels and a title.  

Use the `plt.subplot` method if you can, but if you can't, resort to the `plt` syntax.


```python

x = facebook['Post Month'].value_counts().sort_index().index
y = facebook['Post Month'].value_counts().sort_index().values

fig, ax = plt.subplots(figsize=(10,5))
ax.bar(x,y)
ax.set_title('Facebook Posts Per Month')
ax.set_xlabel('Month')
ax.set_ylabel('Number of Posts')
ax.set_xticks(range(1,13))
ax.set_xticklabels(list('JFMAMJJASOND'));
```

# Task 7

Create a scatter plot that shows the correlation between total interactions and likes.


```python

x = facebook['Total Interactions']
y = facebook['like']


fig, ax = plt.subplots(figsize=(10,5))
ax.scatter(x,y)
ax.set_title('Correlation Between Likes\n and Total Interactions\of Facebook Posts')
ax.set_xlabel('Total Interactions')
ax.set_ylabel('Likes');
```

<a id='datastructures'></a>
# Data Structures

For this next section, we will explore a nested dictionary that comes from the Spotify API.  

The `data` variable below contains 6 separate pings, each of which returns a list of the top 20 songs streamed on a given day.


We will work only with the first response.

# Task 8

Explore the `first_response` dictionary and find how to access the list of twenty songs.
Assign the list to the variable `first_twenty_songs`.
Hint: print out the keys at each level with .keys().


```python
first_twenty_songs = first_response['albums']['items']
```

# Task 9

Create a list of **track names** of all twenty songs using a for loop or list comprehension.


```python
track_names = []
for track in first_twenty_songs:
    track_names.append(track['name'])
    

```

# Task 10

Create a dictionary called `song_dictionary` which consists of each track name `string` as a key and a `list` of artists associated with each track as a value.


```python
song_dictionary = {}

for record in first_twenty_songs:
    artist_list = []
    for artist in record['artists']:
        artist_list.append(artist['name'])
    song_dictionary[record['name']] = tuple(artist_list)
    
song_dictionary
```

# Task 11

Create a function with takes an **artist name** and the **song_dictionary** as arguments, and returns a `list` of songs written by that artist. 


```python

def find_song_by_artist(artist_name, song_dictionary):
    
    '''
    Parameters:
    arist_name: a string of an artist's name to be used to search the dictionary
    song_dictionary:  a dictionary of top_twenty songs with song name as keys and a list of 
    artist names as values
    
    Returns:
    A list of songs which the given artist appeared on
    '''
    
    song_list = []
    
    for song in song_dictionary:
        if artist_name in song_dictionary[song]:
            song_list.append(song)
    
    return song_list
```
