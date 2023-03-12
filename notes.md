# Blog Post
This is my commentary on the task I do while running the repo Recs-at-Reasonable-scal from Jacopo :)

* First I am readin the blog attached to the repo [NVIDIA Merlin meets the MLOps eco system](https://medium.com/nvidia-merlin/nvidia-merlin-meets-the-mlops-ecosystem-building-a-production-ready-recsys-pipeline-on-cloud-1a16c156166b)

* Open source communities like Hugging face is one of the main factor for growth in nlp. Example their apis helped in standardizing the way to productionize the models on cloud becaus of their open nature.

**The Hugging Face moment for Recommender Systems**

* Recommender resist this moment becaus the use case are more specific in nature compared to language. But due to wide spread deep learning approaches in rec sys field. Rec sys is also close to it "Hugging face moment". Nvidia Merlin is one of the drivers by providing easy to use apis for productionizing the rec sys models.

In this blog it's about how to use NVIDIA-Merlin APIs in the wild fire spread of MLOps/DataOps tools

**A tale of two winds**

* It' s all about moving away from research pieline with static file and notebooks to consdtantly updating data pipline with dynamic data and models. 

* This blog is about how a small team of engineers can understand and maintain end-to-end recommendation system, with key takeaways of *modelling* through Merlin and *Infrastruture* through Metaflow.



**End-to-end fashion recommendations**
Use public H&M dataset with offline training and offline serving reccomendation use case.

* **User-item:**  At test, based on user identifier we need to suggest K items that user is likely to buy. We can evaluate based on previous purchases.
* **Offline training and offline serving:** RUn pipeline nightly and prepare predictions in batch.

![Batch Predictions](https://miro.medium.com/v2/resize:fit:828/0*ApQqkIX63Y8Txf51)

**4 Steps of pipeline**

|![](https://miro.medium.com/v2/resize:fit:828/format:webp/1*SSkgIgkLzwwkIbOsliKepg.png)|
|:--:|
|The four main steps in the pipeline — DataOps, Training, Testing and Serving — together with the tools and libraries involved.|


* DataOps: dbt builds DAG of transformations from SQL queries, allowing to run queries of millions of rows leveraging scalability of compute of modern data warehouses.


# Deploying Infrastructure for Metaflow


# Snowflake
* Downlad dataset from kaggle [https://www.kaggle.com/general/156610]
    * pip install -q kaggle
* kaggle competitions download -c h-and-m-personalized-fashion-recommendations
* download only some files
* kaggle competitions download -f {filename}.csv  -c h-and-m-personalized-fashion-recommendations
* Unzip the csv.zip files from H&M files
* Facing the error: snowflake.connector.errors.ForbiddenError: 250001 (08001): Failed to connect to DB. Verify the account name is correct: WD85146.snowflakecomputing.com:443. 000403: HTTP 403: Forbidden

# DBT

What to add in profiles.yml? [TODO]

```
nvidia-snowflake:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: "mjcivuj-wd85146"

      # User/password auth
      user: "saikalyan9981"
      password: "--"

      role: "ACCOUNTADMIN"
      database: "EXPLORATION_DB"
      warehouse: "COMPUTE_WH"
      schema: "HM_POST"
      threads: 1
      client_session_keep_alive: False
      query_tag: "Recsys"

      # optional
      connect_retries: 0 # default 0
      connect_timeout: 10 # default: 10
      retry_on_database_errors: False # default: false 
      retry_all: False  # default: false
```

* Checking out video ![Post modern Data Stack](https://www.youtube.com/watch?v=5kHDb-XGHtc&ab_channel=JacopoTagliabue)
* dbt -d  run --profiles-dir ./ (needde to pass profiles.ymlk directory if it's not in home)
* assertion error too many messages received before intialisation (change to python 3.9 and it's working now)
* Schema.yml is representing the flow?
    * artciles_staging,  customers_staging, transactions_staging - M ap data in json to fully flat structure
    * image_staging - S3 url of products with naming convention ??
    * articles_metadata - join metadata with image url to get full product metadata 
    * dedup_transactions - for each user and article keep the last purchase
    * joined_datafrtame - join the dedup purchases with articles and customers metadata
    * filtered_dataframe - filter to a final datasets in which users have at least 5 training set purchases.


# Run the flow:
AWS_PROFILE="Sai Kalyan Siddanatham" python my_merlin_flow.py run --max-workers 1 --with card

