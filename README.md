# recommendation-system
A recommendation system developed for the GfK NextGen Data Science Hackathon that relies on predictive analytics to suggest products to ecommerce shoppers, based on their purchase histories and personal characteristics

## A recommendation service that suggest the most relevant brands to 4 million customers based on their purchase history of 112 different cereal brands.
### Input
`hhid`
### Output
`ranked list of cereal brands that the user most likely want to put in his/her basket`
### Steps:
- Clean and prepare the data for summary of cereal purchases from 01/01/2019 to 12/31/2021 with 14 million records
- Apply Item Based Collaborative Filtering Model to predict user's preference of cereal products they have never purchased and recommend the 5 most relevant brands.

### Collaborative Filtering Model
- Collaborative Filtering is a technique or a method to predict a user's tatse and find the items that a user might prefer on the basis of information  collected from various other users having similar tatses or preferences.
- Does not require any detail information about the product except for the purchase history of the product by the users.

### In general,
- We recommend a user items that he/she has never bought before but they were relevant to the products the user has purchased in the past.

- The two most popular forms of collaborative filtering are:
    - **User Based**: Measure the **similarity among users based on their purchase history**. We find the target user's preference of the missing item with the help of these users.
    - **Item Based**: Measure **how similar a product is to another product**. We find the target user's preference of the missing item with the help of the preference given to the other items by other users.
    
### Decision

We decide to use the **Item-to-Item Based Collborative Filtering Model** since we have more users than items.

User preferences and liking may also change over time so an item-to-item approach can tackle this problem more efficiently.

### Steps:
1. Create an item-to-item similarity matrix. This is used to measure how similar a product is to another product. We choose `cosine` similarity as our main method.
     - To compare item A and B, we look at all customers who have rated both these items. We create two item-vectors, v1 for item X, and v2 for item Y, and find the `cosine` angle/distance between these vectors. 
     - The similarity is defined by the following formula:

        + **similarity(A, B)** = $cos(\vec{A},\vec{B}) = \frac{\vec{A}.\vec{B}}{||\vec{A}||.||\vec{B}||} = \frac{\sum_u freq_{u, A}.freq_{u, B}}{\sqrt{\sum_u freq^2_{u,A}}\sqrt{\sum_u freq^2_{u, B}}}$

    - As the vectors get closer, the angle will be smaller and the `cosine` will be larger.
    - Cosine similarity gives value between -1 and 1
        - Close to 1: Two items are strongly similar to each other
        - Close to -1: Two items are strongly not similar to each other
        - CLose to 0: There is not much correlation between 2 items
        
2. Calculating the prediction of a user u for item i
    - We use the items (already rated by the user) that are most similar to the missing item to generate rating. Specifically, we try to generate predictions based on the ratings of similar products.
    - We use a formula which computes the rating for a particular item using weighted sum of the ratings of the other similar products
    
        + **prediction(u, i)** = $\frac{\sum_j (freq_{u, j}).similarity_{i,j}}{\sum_j similarity_{i,j}}$
