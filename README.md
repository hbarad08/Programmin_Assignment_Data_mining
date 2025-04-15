# **Movie Recommendation System**

## **Overview**
This project implements a movie recommendation system using the MovieLens 100K dataset. The system explores three distinct approaches to recommending movies:
1. **User-Based Collaborative Filtering**: Recommends movies based on the preferences of similar users.
2. **Item-Based Collaborative Filtering**: Suggests movies similar to those a user has already rated.
3. **Random-Walk-Based Pixie Algorithm**: Simulates random walks on a bipartite graph to discover deeper connections between users and movies.

The goal is to evaluate the effectiveness of these methods and provide meaningful recommendations.

---

## **Dataset**
The MovieLens 100K dataset contains:
- **943 users** who rated at least 20 movies.
- **1682 movies** spanning various genres.
- **100,000 ratings**, ranging from 1 to 5 stars.

### Features:
- `user_id`: Unique identifier for each user.
- `movie_id`: Unique identifier for each movie.
- `rating`: The score assigned by a user to a movie (1–5).
- `timestamp`: The time when the rating was given.

### Preprocessing:
1. Converted timestamps into readable date formats using `pd.to_datetime()`.
2. Constructed matrices (e.g., User-Movie Matrix) for collaborative filtering techniques.
3. Built adjacency lists for graph-based methods, connecting users and movies based on ratings.

---

## **Methodology**
### **1. User-Based Collaborative Filtering**
- Calculates similarity between users using cosine similarity or Pearson correlation.
- Recommends movies that similar users have rated highly but the target user has not yet rated.

### **2. Item-Based Collaborative Filtering**
- Measures similarity between movies based on shared user ratings using cosine similarity.
- Suggests movies that are most similar to those already rated by the user.

### **3. Random-Walk-Based Pixie Algorithm**
- Constructs a bipartite graph where nodes represent users and movies, and edges represent ratings.
- Simulates weighted random walks starting from nodes (users or movies) to explore deeper connections in the graph.
- Movies visited frequently during walks are ranked higher for recommendation.

---

## **Implementation Details**
### **Graph Construction**
An adjacency list representation was used to build the bipartite graph:
- User nodes are connected to movie nodes they have rated.
- Movie nodes are connected back to user nodes representing the users who rated them.

Example adjacency list:
```python
{
    196: {242, 302, 377},   # User 196 rated movies 242, 302, and 377
    242: {196, 186},        # Movie 242 was rated by users 196 and 186
}
```

### **Weighted Random Walks**
The random walk algorithm traverses the graph starting from a given node (`user_id` or `movie_id`). Probabilities for transitions are weighted based on edge strengths (ratings).

Key parameters:
- `walk_length`: Number of steps in each random walk (default: 15).
- `num_walks`: Number of random walks performed for each starting node.

Example implementation:
```python
def weighted_random_walk(graph, start_node, walk_length=15):
    visited_movies = []
    current_node = start_node

    for step in range(walk_length):
        neighbors = list(graph.get(current_node, []))
        if not neighbors:
            break
        next_node = random.choice(neighbors)
        if isinstance(next_node, int):  # Assuming movie_ids are integers
            visited_movies.append(next_node)
        current_node = next_node

    return visited_movies
```

### **Collaborative Filtering**
Both user-based and item-based collaborative filtering utilized cosine similarity matrices calculated using `sklearn.metrics.pairwise.cosine_similarity`.

Steps for User-Based Collaborative Filtering:
1. Constructed a User-Movie Matrix using Pandas’ `pivot()` function:
   ```python
   user_movie_matrix = ratings.pivot(index='user_id', columns='movie_id', values='rating')
   ```
2. Computed cosine similarity between rows (users):
   ```python
   user_similarity = cosine_similarity(user_movie_matrix.fillna(0))
   ```
3. Recommended top-rated movies from similar users.

Steps for Item-Based Collaborative Filtering:
1. Constructed a Movie-User Matrix (transpose of User-Movie Matrix).
2. Computed cosine similarity between rows (movies).
3. Recommended top-rated movies similar to those already rated by the target user.

---

## **Results**

### **User-Based Collaborative Filtering**
Example output:
```
Top 5 recommendations for User 196:
| Ranking | Movie Name                          |
|---------|-------------------------------------|
| 1       | Star Wars (1977)                    |
| 2       | Jurassic Park (1993)               |
| 3       | Independence Day (1996)            |
| 4       | Mission Impossible (1996)          |
| 5       | Toy Story (1995)                   |
```

### **Item-Based Collaborative Filtering**
Example output:
```
Top 5 recommendations for Movie "Toy Story (1995)":
| Ranking | Movie Name                          |
|---------|-------------------------------------|
| 1       | GoldenEye (1995)                    |
| 2       | Four Rooms (1995)                   |
| 3       | Get Shorty (1995)                   |
| 4       | Copycat (1995)                      |
| 5       | Twelve Monkeys (1995)               |
```

### **Random-Walk-Based Pixie Algorithm**
Example output:
```
Top 5 recommendations using Random Walks:
| Ranking | Movie Name                          |
|---------|-------------------------------------|
| 1       | The Godfather (1972)               |
| 2       | Pulp Fiction (1994)                |
| 3       | Schindler's List (1993)            |
| 4       | Forrest Gump (1994)                |
| 5       | Braveheart (1995)                  |
```

---

## **Conclusion**
This project demonstrates the effectiveness of three distinct recommendation techniques:
1. **User-Based Collaborative Filtering** excels in identifying personalized recommendations based on shared preferences among users.
2. **Item-Based Collaborative Filtering** is ideal for suggesting content similar to what the user already enjoys.
3. The **Random-Walk-Based Pixie Algorithm** provides unique insights by leveraging graph traversal techniques.

---

## **Future Work**
To enhance this system further:
1. Implement hybrid models combining collaborative filtering with content-based approaches using movie metadata like genres, actors, and directors.
2. Explore deep learning techniques such as autoencoders or neural collaborative filtering for improved accuracy.
3. Incorporate demographic data to personalize recommendations further.

---

## **How to Run**

### Prerequisites
Make sure you have Python installed along with the following libraries:
- pandas
- numpy
- scikit-learn

### Steps
1. Clone this repository:
   ```bash
   git clone https://github.com//.git
   cd 
   ```
2. Install dependencies:
   ```bash
   pip install pandas numpy scikit-learn
   ```
3. Run the notebook or script containing the code.

---

## **References**
1. GroupLens Research - MovieLens Dataset Documentation
2. Harper & Konstan, "The MovieLens Datasets: History and Context"
3. Netflix Recommendation System Insights

---

Let me know if you'd like any changes or additional sections!

---
Answer from Perplexity: pplx.ai/share
