# Deploying Infrastructure for Metaflow


# Snowflake
* Downlad dataset from kaggle [https://www.kaggle.com/general/156610]
    * pip install -q kaggle
* kaggle competitions download -c h-and-m-personalized-fashion-recommendations
* download only some files
* kaggle competitions download -f {filename}.csv  -c h-and-m-personalized-fashion-recommendations
* Activate local.env file
* Facing the error: snowflake.connector.errors.ForbiddenError: 250001 (08001): Failed to connect to DB. Verify the account name is correct: WD85146.snowflakecomputing.com:443. 000403: HTTP 403: Forbidden