# Ecommerce-product-recommendation-system

Product Recommendation System is a machine learning-based project that generates personalized product recommendations by analyzing users' browsing activities and purchase history. The system combines collaborative filtering and content-based recommendation techniques to understand user preferences and suggest relevant products. The main objective of this project is to enhance the shopping experience for customers while helping e-commerce businesses improve customer engagement and boost sales.

## Dataset

This project utilizes the Amazon Electronics Ratings Dataset, which contains user ratings for electronic products. The dataset does not include column headers. To minimize potential bias, each user and product is represented using a unique identifier instead of names or any other personally identifiable information.

- You can find the dataset here - https://www.kaggle.com/datasets/vibivij/amazon-electronics-rating-datasetrecommendation/download?datasetVersionNumber=1
- You can explore more Amazon datasets here - https://jmcauley.ucsd.edu/data/amazon/

## Approach

### **1) Rank Based Product Recommendation**

**Objective -**

- Recommend products that have received the highest number of ratings.
- Suggest the most popular products to new users.

**Outputs -**

- Recommend the top 5 products with a minimum of 50/100 ratings or interactions.

**Approach -**

- Compute the average rating for each product.
- Calculate the total number of ratings received by every product.
- Create a dataframe using these values and sort it based on the average rating.
- Develop a function to retrieve the top 'n' products that satisfy the minimum interaction threshold.

### **2) Similarity-based Collaborative Filtering**

**Objective -**

- Generate personalized product recommendations by identifying users with similar interests.

**Outputs -**

- Recommend the top 5 products based on the interactions of users with similar preferences.

**Approach -**

- Since `user_id` is stored as an object, it is converted into integer values ranging from 0 to 1539 for easier processing.
- A function is created to identify similar users:
  1. Calculate the cosine similarity between the selected user and every other user in the interaction matrix, store the similarity scores in a list, and sort them.
  2. Extract the similar users along with their corresponding similarity scores.
  3. Remove the selected user and return the remaining similar users.
- A recommendation function is then implemented:
  1. Invoke the similar users function to retrieve users with the highest similarity.
  2. Identify the products already interacted with by the target user (`observed_interactions`).
  3. For each similar user, retrieve products they have interacted with that the target user has not.
  4. Return the specified number of recommended products.

### **3) Model-based Collaborative Filtering**

**Objective -**

- Provide personalized recommendations based on users' historical interactions while effectively handling sparsity and scalability challenges commonly found in collaborative filtering systems.

**Outputs -**

- Recommend the top 5 products for a specific user.

**Approach -**

- Convert the user-product rating matrix into a CSR (Compressed Sparse Row) matrix to reduce memory consumption and improve computational efficiency by storing only non-zero values.
- Apply Singular Value Decomposition (SVD) to reduce the dimensionality of the sparse matrix into 50 latent features.
- Generate predicted ratings for all users by reconstructing the rating matrix using the U, Sigma, and Vt matrices obtained from SVD.
- Store the predicted ratings in a dataframe with the same columns as the original interaction matrix, where each row represents a user.
- Develop a recommendation function that:
  1. Retrieves the user's existing ratings from the interaction matrix.
  2. Obtains the user's predicted ratings from the predictions matrix.
  3. Combines the actual and predicted ratings into a dataframe.
  4. Maps the corresponding product names to the dataframe.
  5. Filters out products that the user has already rated.
  6. Sorts the remaining products based on predicted ratings in descending order.
  7. Returns the top `num_recommendations` products.
- Evaluate the recommendation model:
  1. Calculate the average of all actual product ratings.
  2. Calculate the average of all predicted ratings.
  3. Create a dataframe (`rmse_df`) containing both actual and predicted average ratings.
  4. Compute the Root Mean Squared Error (RMSE) by taking the square root of the mean squared difference between the actual and predicted ratings.

> The `squared` parameter in the `mean_squared_error()` function determines whether the output is Mean Squared Error (MSE) or Root Mean Squared Error (RMSE). By setting `squared=False`, the function returns RMSE, which is calculated by taking the square root of the MSE. RMSE is used in this project to evaluate how closely the predicted ratings match the actual user ratings.
