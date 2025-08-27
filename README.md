
-----

# MovieLens Analytics Platform: ETL, Data Warehouse & Recommendation Engine

## Overview

This project demonstrates an end-to-end data engineering and machine learning pipeline using PySpark. It processes the classic MovieLens 1M dataset, which contains one million movie ratings from 6,040 users for 3,883 movies.

The core of the project involves:

1.  **ETL (Extract, Transform, Load):** Ingesting raw `.dat` files, cleaning and transforming the data.
2.  **Data Warehousing:** Building a star schema with dimension and fact tables to support efficient analytics.
3.  **Data Analysis:** Using Spark SQL to query the warehouse and uncover insights.
4.  **Machine Learning:** Building a collaborative filtering recommendation system to predict user ratings.

The entire pipeline is developed within a PySpark environment, showcasing best practices for distributed data processing, performance optimization, and model building.

## Features

  * **Robust ETL Pipeline:** Parses semi-structured, `::` delimited text files into structured Spark DataFrames.
  * **Dimensional Data Model:** Implements a star schema with 4 dimension tables (`dim_movies`, `dim_users`, `dim_genres`, `dim_movie_genres`) and 1 fact table (`fact_ratings`) for optimized analytical queries.
  * **Performance Optimization:** Employs Parquet as the storage format and partitions the `fact_ratings` table by `year` and `month` to accelerate time-based queries.
  * **In-depth SQL Analysis:** Includes several analytical queries to identify top-rated movies, genre popularity over time, and most active users.
  * **Movie Recommendation Engine:** Trains an Alternating Least Squares (ALS) model on the dataset to generate personalized top-5 movie recommendations for each user, achieving an **RMSE of 0.87**.
  * **Data Quality Assurance:** Includes validation steps to check for nulls and ensure referential integrity between fact and dimension tables.

## Tech Stack

  * **Framework:** Apache Spark (via PySpark)
  * **Language:** Python
  * **Libraries:**
      * `pyspark.sql` for data manipulation and ETL.
      * `pyspark.ml` for building the recommendation model.
  * **Data Format:** Parquet for curated data storage.
  * **Environment:** Google Colab / Jupyter Notebook

## Data Schema

The ETL process transforms the raw data into the following star schema:

#### Fact Table

  * `fact_ratings`
      * `user_id` (int)
      * `movie_id` (int)
      * `rating` (float)
      * `rating_ts` (timestamp)
      * `year` (int) - *Partition Key*
      * `month` (int) - *Partition Key*
      * `day` (int)

#### Dimension Tables

  * `dim_movies`
      * `movie_id` (int)
      * `title` (string)
      * `release_year` (int)
  * `dim_users`
      * `user_id` (int)
      * `gender` (string)
      * `age_code` (int)
      * `occupation_code` (int)
      * `zip_code` (string)
      * `age_bucket` (string)
  * `dim_genres`
      * `genre_id` (int)
      * `genre` (string)
  * `dim_movie_genres` (Bridge Table)
      * `movie_id` (int)
      * `genre_id` (int)

## How to Run

1.  **Clone the Repository:**

    ```bash
    git clone <your-repo-url>
    cd <your-repo-name>
    ```

2.  **Set up Environment:**
    This project is designed to run in a PySpark environment like Google Colab. Ensure you have PySpark installed.

    ```bash
    pip install pyspark
    ```

3.  **Run the Notebook:**
    Open the `Movielens.ipynb` notebook in a Jupyter or Google Colab environment and execute the cells sequentially. The notebook handles:

      * Downloading and extracting the MovieLens 1M dataset.
      * Setting up file paths for raw and curated data.
      * Executing the ETL and data modeling steps.
      * Writing the final tables to Parquet files.
      * Running analytical queries.
      * Training and evaluating the recommendation model.

## Analytical Insights Examples

The project answers several key business questions, including:

  * **Top 10 Highest-Rated Movies (min. 100 ratings):**

      * Identifies critically acclaimed films like *"Seven Samurai"* (4.56 avg. rating) and *"The Shawshank Redemption"* (4.55 avg. rating).

  * **Most Reviewed Movies:**

      * Finds films with the highest engagement, such as *"American Beauty"* with 3,428 ratings.

  * **Genre Popularity Over Time:**

      * Analyzes the total number of ratings per genre for each year, showing trends in viewer preferences.

  * **Most Active Users:**

      * Identifies power users based on the total number of ratings submitted.

## Machine Learning Model: Recommendation Engine
* **Algorithm**: 
    * Alternating Least Squares (ALS), a collaborative filtering method.

* **Objective**: 
    * Predict user ratings for movies they haven't seen.

* **Performance**: 
    * The model was trained on 80% of the data and achieved an RMSE of 0.87 on the remaining 20% test set, indicating strong predictive accuracy.

* **Output**: 
    * The model can generate the top N movie recommendations for any given user.
