MovieLens Analytics Platform with PySpark
This project demonstrates an end-to-end data engineering and machine learning pipeline using PySpark. It processes the MovieLens 1M dataset to build a star schema data warehouse, derives analytical insights with Spark SQL, and trains a collaborative filtering model to generate personalized movie recommendations.

Key Features
End-to-End ETL Pipeline: Ingests, cleans, and transforms over 1 million user ratings from raw .dat files into a structured, query-optimized format.

Dimensional Data Warehouse: Implements a star schema with 1 fact table and 4 dimension tables, ideal for analytical queries.

In-Depth Data Analysis: Leverages Spark SQL to uncover trends, such as top-rated movies, genre popularity, and user demographics.

Machine Learning Recommender: Builds a movie recommendation engine using PySpark MLlib's Alternating Least Squares (ALS) algorithm, achieving an RMSE of 0.87.

Performance Optimization: Employs Parquet for efficient columnar storage and partitions the primary fact table by year and month to accelerate time-based queries.

Technical Implementation
1. ETL and Data Processing
The pipeline begins by ingesting three raw data files (ratings.dat, users.dat, movies.dat). Using PySpark DataFrames, the semi-structured, ::-delimited text is parsed into structured tables.

Key transformations include:

Converting the Unix timestamp in ratings.dat to a standard timestamp format.

Extracting the movie's release year from its title using regex.

Exploding the pipe-separated genres string into a normalized, one-to-many format.

2. Data Modeling: Star Schema
To support efficient analytics, the data is modeled into a star schema. This design separates the core measurements (the "facts") from their descriptive context (the "dimensions").

fact_ratings: The central fact table containing foreign keys to the dimension tables and the core metric (rating). Contains 1,000,209 records.

dim_users: Dimension table with descriptive information about each of the 6,040 users (gender, age, occupation).

dim_movies: Dimension table for the 3,883 movies (title, release year).

dim_genres: Dimension table for the 18 unique movie genres.

dim_movie_genres: A bridge table linking movies to genres, containing 6,408 relationships.

The final, curated tables are written to disk in the efficient Parquet format and partitioned by year and month for performance.

3. Data Analysis with Spark SQL
With the data warehouse in place, Spark SQL is used to query and analyze the data. The project answers several key business questions:

What are the highest-rated movies?

Finding: "Seven Samurai" is the top-rated film with an average score of 4.56 (among movies with >100 ratings).

Who are the most active users?

Finding: User ID 4169 is the most active, having rated 2,314 movies.

What are the most popular movies by rating count?

Finding: "American Beauty" is the most-rated movie with 3,428 total ratings.

4. Recommendation Engine with ALS
A collaborative filtering model is built to predict user ratings and generate recommendations.

Algorithm: Alternating Least Squares (ALS) from Spark's MLlib.

Training Data: The model was trained on an 80/20 split of the 1M ratings.

Hyperparameters: rank=20, maxIter=10, regParam=0.1.

Performance: The model achieved a Root Mean Square Error (RMSE) of 0.87 on the validation set.

Output: The trained model can generate the top 5 movie recommendations for any given user.

🔧 How to Run
This project is designed to run in a PySpark environment like Google Colab or a local Jupyter Notebook setup with Spark installed.

Clone the repository:

git clone https://github.com/your-username/movielens-pyspark-project.git
cd movielens-pyspark-project

Launch the Notebook: Open Movielens.ipynb in your Jupyter or Colab environment.

Execute the Cells: Run the notebook cells sequentially. The script will automatically:

Download the MovieLens 1M dataset.

Execute the entire ETL and data modeling pipeline.

Save the curated data warehouse tables.

Perform the SQL-based analysis.

Train and evaluate the ALS recommendation model.

Technologies Used
Apache Spark:

PySpark API

Spark SQL

Spark MLlib

Python: For scripting and data manipulation.

Pandas: (Implicitly via Spark) for DataFrame concepts.

Jupyter/Google Colab: For interactive development and presentation.
