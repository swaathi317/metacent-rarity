# metacent-rarity

### Basic Setup

The local machine (Linux) requires JAVA, Hadoop and Spark to be installed and configured.

Add environment variables if not have been configured:

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export SPARK_HOME=/home/{user}/spark-3.1.2-bin-hadoop3.2/
export PYSPARK_PYTHON=python3
```

Start the local ssh :

```bash
sudo service ssh start
```

Start NameNode daemon and DataNode daemon:

```bash
hadoop/hadoop-3.3.1/sbin/start-dfs.sh
```

Clone the project directory:

https://github.com/smithakolan/NFT-Big-Data-Analysis.git

## Data Collection

#### Extraction of Collection Stats

Command to run file: Data_Collection\collect_stats.py

```bash
time ${SPARK_HOME}/bin/spark-submit Data_Collection\collect_stats.py
```

The program produces HDFS folder called DAppStats

<br /> <br />

#### Extraction of NFTs

Command to run file: Data_Collection\getAssets.py

```bash
time ${SPARK_HOME}/bin/spark-submit Data_Collection\getAssets.py
```

The program produces a HDFS folder called nftdata. The file from this folder is acquired.

<br /><br />


#### Extraction of Google Trends

Google Trends is acquired by using a library called PyTrends. It needs to be installed to the project folder before data collection.

```bash
pip install pytrends
```

Command to run file: Data_Collection\getGoogleTrends.py

```bash
python Data_Collection\getGoogleTrends.py
```

The program produces a googleTrends folder. It contains google trends of all dapps.

<br /><br />

#### Extraction of Twitter Account details

Twitter account details is acquired using a library called tweepy. It needs to be installed to the project folder before data collection.

```bash
pip install tweepy
```

Command to run file: Data_Collection\getTwitterDapps.py

```bash
python Data_Collection\getTwitterDapps.py
```

The program produces a JSON file called twitterDapps.

<br /><br />

## Data Cleaning and Integration

#### Transformation, Cleaning and Integration of Dapps

Command to run file: Data_Integration_Cleaning\transformCleanDapps.py

```bash
time ${SPARK_HOME}/bin/spark-submit Data_Integration_Cleaning\transformCleanDapps.py
```

The program produces a JSON file called dapps.json

<br /><br />

#### Transformation, Cleaning and Integration of NFTs

Command to run file: Data_Integration_Cleaning\transformCleanNFT.py

```bash
time ${SPARK_HOME}/bin/spark-submit Data_Integration_Cleaning\transformCleanNFT.py
```

The program produces a HDFS folder called cleanednfts.

<br /><br />

#### Transformation, Cleaning and Integration of Google Trends

Command to run file: Data_Integration_Cleaning\transformCleanGoogleTrends.py

```bash
python Data_Integration_Cleaning\transformCleanGoogleTrends.py
```

The program produces a folder called cleanedGoogleTrends.

<br /><br />

#### Transformation, Cleaning and Integration of Twitter details

Command to run file: Data_Integration_Cleaning\transformCleanTwitterDapps.py

```bash
python Data_Integration_Cleaning\transformCleanTwitterDapps.py
```

The program produces a file called cleanedTwitterDapps.csv

<br /><br />

#### Cleaning and Processing of Dapps

Command to run file: Data_Integration_Cleaning\top10Dapps.py

```bash
python Data_Integration_Cleaning\top10Dapps.py
```

The program produces a file called top_dapps.json

<br /><br />

## Data Loading

#### An AWS account has to be created and a Administrator User account should be created before proceeding to the next step. After creation, the AWS ACCESS_ID and ACCESS_KEY should be added to the following files of the project.

#### Loading of Dapps to Database

Command to run file: Data-Loading\loadDapps.py

```bash
python Data-Loading\loadDapps.py
```

<br /><br />

#### Loading of NFTs to Database

Command to run file: Data-Loading\loadNFT.py

```bash
python Data-Loading\loadNFT.py
```

<br /><br />
#Analysis

## Rarity Factor
Rarity is a tremendously important metric that defines the price of an NFT. It can be determined in a variety of different ways. In our approach on rarity, we look at the attributes of each NFT and algorithmicly calculated how ubiquitous or rare each of those attributes are. Based on this, we were able to come up with a rarity factor for each NFT. We collected the data needed from Opensea, one of the largest NFT marketplaces.

## Transaction Maps
Transaction are a excellent way to visualize the NFT market and see the spread of NFT across wallets. For this we collected transactional data from the ethereum blockchain. By nature, blockchain allow anonyminty, and hence we don't have information of the owners of each wallet address. We chose to represent the NFT transaction data in a multi- directed weighted graph MDG(V,E) with a set of nodes V and edges E. Each node represent a unique wallet address and each directed edge represents a single transaction. Each node is weighted on the number of incoming edges it has, hence the number of NFTs it has. Each edge is weighted base on the cost of the transaction, i.e the price paid for the NFT. Across the top collections we have chosen, we observed similiar trends in that there were a few key players who had alot of NFTs in their wallets.

We tried numerous graphing libraries like NetworkX and Graph-tools, and we ultimately ended up choosing dephi as it is a powerful graphing software that is capabale with dealing with a large number or nodes and edges while at the same time creating high quality visualizations.

## Identifying Communities
Making use of the transaction data, we computed modularity of each network which gives us an idea if communities exist. Communites refer to wallets that are frequently trading between each other. 

## Price evaluation with XGBoost
Another important feature which provides a lot of value for NFT investors is a price evaluation of NFTs. This is a feature which will inform investors if an NFT is under-valued or over-valued, based on looking at factors like rarity, last sold price and also the number of times an NFT has been sold. We believe these factors really play a major role in determining the price of an NFT. The number of times an NFT has been sold is indicative of the interest in it. The last sold price gives a really good measure of what price a particular NFT can be sold for in the future. 

## Website for results
Website URL: https://metacent.cyclic.app/home
<br/>
Website Git Repo: https://github.com/smithakolan/metacent-website




