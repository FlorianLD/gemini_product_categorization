# About

Product categorization project, using the Gemini embeddings API to assign subcategories to products based on existing subcategories, and performed through embeddings comparisons.<br>

# Objective

The objective was to understand how we can reliably assign subcategories to new products using existing product data through embeddings.<br>
The categorization is based on embeddings similarities: the subcategory assigned to a new product is the subcategory of the existing product whose embeddings are the closest to the new product's embeddings.<br>

# Embeddings API

In this project, the embeddings are product names transformed in lists of float values, known as vectors.<br>
The closer two values' embeddings are, the closer their semantic meaning.<br>
For this reason, embeddings can be used to group data based on their semantic similarity.<br>
For more details, refer to [Google's embeddings guide](https://ai.google.dev/gemini-api/docs/embeddings).

The Gemini embeddings API offers the `task_type` parameter to enable the user to specify what the embeddings will be used for.<br>

![Test](/task_types.png)

In this project, two task types were used to compare their reliability:
- semantic similarity
- classification

For a same value, different task types will produce different embeddings.<br>
See the [TaskType API reference page](https://ai.google.dev/api/rest/v1/TaskType)

# Testing plan

To check the reliability of this process, tests were run on products that already had a subcategory.<br>
Pass/Fail conditions are as follows: 
- Pass: the new subcategory assigned to a product is identical to its default subcategory
- Fail: the new subcategory assigned to a product is different from its default subcategory

See the testing steps below :

1) Load source file with all the products and their subcategory
2) File is divided in 2 parts:
    - dataset, used as a baseline to assign a new subcategory to new products
    - testing subset, containing products with existing subcategories, used to check if the new subcategory assigned is identical to the existing subcategory
3) Generate embeddings for each product of the dataset
4) Generate embeddings for the product of the testing subset
5) Compare the embeddings of the testing subset product to the embeddings of the dataset products
6) Return the dataset product with the closest embeddings to the embeddings of the testing subset product
7) Assign the returned dataset product's subcategory to the testing subset product

See the simplified diagram below 

![Test](/testing_diagram.png)

In this simplified diagram, we can see the test failed for Product 10 since the new subcategory returned (C) is different from the default subcategory (B).

# Results

The test results indicate that product categorization is more reliable when using the semantic similarity `task_type` compared to the classification `task_type`.<br>

On a sample of 100 products:
- 66 passed the tests with the semantic similarity `task_type`
- 53 passed the tests with the classification `task_type`

Given the small scale of this project, the next step would be to experiment on a larger scale to yield better results by leveraging the following elements:
- large dataset (1M+ products)
- large testing subset (1000+ products)
- batch method to generate embeddings in shorter processing time
- vector database to store/query embeddings

# Output example

In the output example below, we can see that the test has failed for product #6 since the new subcategory is different from the default subcategory.

![Test](/result_example.png)

# Material

The material used is an Amazon product dataset csv file ([source](https://www.kaggle.com/datasets/lokeshparab/amazon-products-dataset)).

![Test](/product_file.png)
