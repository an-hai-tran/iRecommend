# Background of the product
- A **Recommendation Service** called *iRecommend* that won First Prize in [GfK NextGen Data Science Hackathon](https://github.com/an-hai-tran/recommendation-system). It
relies on predictive analytics to suggest products to ecommerce shoppers, based on their purchase histories and personal characteristics
- The presentation on this product can be viewed at [Slides](https://docs.google.com/presentation/d/1fV6T4zIwfkVzCLKraK0fWkAPJSzCXeoEV18hs4MF5cY/edit?usp=sharing)

## A recommendation service that suggest the most relevant brands to customers based on their purchase history of different cereal brands.
### Input
`hhid`
### Output
`ranked list of cereal brands that the user most likely want to put in his/her basket`
### Process:
- Clean and prepare the data for summary of cereal purchases from 01/01/2019 to 12/31/2021 with 14 million records
- Apply Item Based Collaborative Filtering Model to predict user's preference of cereal products they have never purchased and recommend the 5 most relevant brands.

### Details about Collaborating Filtering Model and decision to use it
**Definition**
- Collaborative Filtering is a technique or a method to predict a user's tatse and find the items that a user might prefer on the basis of information  collected from various other users having similar tatses or preferences.
- Does not require any detail information about the product except for the purchase history of the product by the users.

**Application**
- We recommend a user items that he/she has never bought before but they were relevant to the products the user has purchased in the past.

- The two most popular forms of collaborative filtering are:
    - **User Based**: Measure the **similarity among users based on their purchase history**. We find the target user's preference of the missing item with the help of these users.
    - **Item Based**: Measure **how similar a product is to another product**. We find the target user's preference of the missing item with the help of the preference given to the other items by other users.
    
**Decision**
- We decide to use the **Item-to-Item Based Collborative Filtering Model** since we have more users than items.

- User preferences and liking may also change over time so an item-to-item approach can tackle this problem more efficiently.

### Implementation:
1. Create an item-to-item similarity matrix. This is used to measure how similar a product is to another product. We choose `cosine` similarity as our main method.
    - To compare item A and B, we look at all customers who have rated both these items. We create two item-vectors, v1 for item X, and v2 for item Y, and find the `cosine` angle/distance between these vectors. 
    - As the vectors get closer, the angle will be smaller and the `cosine` will be larger.
    - Cosine similarity gives value between -1 and 1
        - Close to 1: Two items are strongly similar to each other
        - Close to -1: Two items are strongly not similar to each other
        - CLose to 0: There is not much correlation between 2 items
        
2. Calculating the prediction of a user u for item i
    - We use the items (already rated by the user) that are most similar to the missing item to generate rating. Specifically, we try to generate predictions based on the ratings of similar products.
    - We use a formula which computes the rating for a particular item using weighted sum of the ratings of the other similar products

## A recommendation system that predict a user's preference of products based on that user’s characteristics
### Input
`Age`, `Gender`, and `Race` of customer

### Output
The probability distribution of different categories that the customer would likely to buy
List of categories:
- `Household washing and cleaning products`
- `Haircare products`
- `Skincare products`
- `Make-up, cosmetics or fragrances`
- `Packaged food and beverages (e.g. groceries, snacks)`
- `Clothing/fashion (e.g. items related to apparel, footwear, or personal accessories)`
- `Smartphones, mobile phones`
- `Small home appliances (e.g. coffee makers, toasters, mixers, blenders, vacuum cleaners, irons)`
- `Major home appliances (e.g. refrigerators, washers & dryers, ovens, dishwashers)`
- `Wearables (e.g. fitness trackers, smart watches)`
- `Computing (e.g. desktop, laptop, notebook, tablet, PCs)`
- `Toys`
- `OTC healthcare (e.g. analgesics, cough/cold, allergy, nutritional supplements, bought without a prescription)`
- `Replacement auto or truck tires`
- `Home furnishings and accessories`
- `TV`

### Process:
- Clean and prepare the data for demographic on how items are shopped for in past 6 months (W21,20,19).
- Apply Bayes' Theorem to predict the likelihood a user of given Gender, Age, and Race would buy a product.

### Steps:
- We can represent each row as **Pr[A], Pr[B], or Pr[C]** with A, B, and C being the **Gender, Age, and Race** of a customer, and **Pr[H]** with H being a particular product.
- We calculate the probability a user with given traits A, B, and C would buy the product H as **Pr[H | A, B, C]**
