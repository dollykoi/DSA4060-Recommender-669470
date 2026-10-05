# DSA 4060 Personalized Movie Recommender

## Student Details
- **Name:** Claire Mwarari
- **Student ID:** 669470
- **Assigned User ID:** 31 *(last two digits 70 → 70 MOD 40 = 30 → 30 + 1 = 31)*

## Project Objective
A streaming company wants a simple personalized movie recommender. This
project analyses the historical ratings of one assigned user (User 31),
identifies their genre preferences from movies they rated highly, and
recommends five movies they have not already rated, using a content-based
approach over movie genres.

## Datasets
- **movies.csv** — the movie catalogue: 36 movies with `movie_id`, `title`
  and pipe-separated `genres` (e.g. `Sci-Fi|Thriller`).
- **ratings.csv** — 440 historical ratings from 40 users: `user_id`,
  `movie_id`, `rating` (0.5–5 scale).

Both files are clean: no missing values were found during inspection.

## Method / Approach
1. **Profile the user:** filtered all ratings to User 31 (10 ratings) and
   joined them with the movie catalogue.
2. **Positive evidence:** movies rated **4.0 or higher** (4 movies) were
   treated as movies the user likes.
3. **Vectorization:** genres were converted to TF-IDF features using
   `TfidfVectorizer` with a custom token pattern that splits on `|`.
4. **Similarity:** a preference centroid was built by averaging the TF-IDF
   vectors of the liked movies; every candidate movie was scored by **cosine
   similarity** to this centroid using scikit-learn.
5. **Filtering:** all 10 movies already rated by User 31 were excluded, and
   the top 5 unseen movies by similarity were returned.

## How to Run the Notebook
1. Requirements: Python 3, Jupyter Notebook / VS Code / Google Colab, and
   the libraries `pandas` and `scikit-learn`.
2. Place `DSA4060_Recommender_669470.ipynb`, `movies.csv` and `ratings.csv`
   in the same folder.
3. Run the notebook top to bottom (Kernel → Restart & Run All). All outputs
   are already executed and visible.

## Recommendation Results
Top 5 recommendations for User 31:

| Rank | Movie | Genres | Why |
|---:|---|---|---|
| 1 | The Shawshank Redemption | Drama | Closest unseen match to the liked profile; Drama appears in all 4 movies the user rated ≥ 4★. |
| 2 | Titanic | Romance\|Drama | Exact genre twin (similarity 1.00) of *The Notebook* (4★). |
| 3 | Creed | Sports\|Drama | Exact twin of *Moneyball* (4★), extending the user's sports-drama taste. |
| 4 | Remember the Titans | Sports\|Drama | Same *Moneyball* match (1.00), ordered by centroid similarity. |
| 5 | Ford v Ferrari | Sports\|Drama | Another *Moneyball* twin (1.00). |

**Interpretation:** User 31 likes Drama (present in every liked movie) with
secondary Comedy and Sports/Romance pairings, and dislikes Action/Sci-Fi
(their lowest ratings). The recommendations are all unseen Drama-led films
that mirror at least one highly rated movie, so they are well supported by
the user's own history.

## Limitation and Suggested Improvement
- **Limitation:** the recommender uses only genre strings and treats a 4★ and
  a 5★ movie equally, so it produces narrow "genre clones" — four of the five
  recommendations are Drama variants — with no diversity or sense of quality.
- **Improvement:** build a **hybrid** system that adds collaborative
  filtering (e.g. SVD/matrix factorization over all 440 ratings) so items
  liked by users with similar taste surface even when genres differ; and/or
  **weight liked movies by rating** when building the preference centroid.
