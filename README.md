
## Introduction
This project involves developing a live product recommendation system tailored to customer preferences, using the Amazon product dataset. The system leverages big data processing, NoSQL databases, machine learning algorithms, web development, and stream processing to deliver personalized product recommendations, thereby enhancing the shopping experience on an e-commerce platform.

## Context and Objectives
n the modern digital landscape, e-commerce platforms play a crucial role in our daily lives, offering a plethora of products and services. However, the sheer volume of options can make it challenging for users to find products that match their interests. This underscores the necessity for an intelligent recommender system that can analyze user data and generate tailored recommendations. The objective of this project is to fulfill this need by creating a product recommender system using the Amazon product dataset. By employing advanced technologies such as Apache Spark for data processing, MongoDB for efficient data storage, machine learning algorithms for model training, Flask for web application development, and Apache Kafka for real-time streaming, the project aims to provide accurate and timely recommendations to users. This project offers a comprehensive opportunity to gain practical experience in big data processing, NoSQL databases, machine learning, web development, and stream processing.

## Data Ingestion
To streamline the data loading process for future sampling needs, a well-defined schema was employed. Given the substantial size of the data (123 GB), a partition key was incorporated during the initial load into MongoDB. This strategic decision enhances efficiency when sampling the data for visualizations and analysis, conserving valuable time and resources.

The partition key option specifies the partition key for writing the DataFrame to MongoDB, which significantly impacts data distribution and organization within MongoDB. By defining the partition key, data is partitioned and distributed across different nodes or shards in the MongoDB cluster. In this case, the asin field was chosen as the partition key, ensuring that records with the same asin value are stored together within the same partition or shard. This choice of partition key can greatly improve query performance, data distribution, and scalability, facilitating efficient sampling and querying of the data. This optimization in data organization and access patterns enables efficient data retrieval, sampling, and subsequent processing.


## Feature Engineering
Feature engineering is a vital step to ensure the optimal performance of machine learning models. This project placed special emphasis on feature engineering to improve the quality and effectiveness of the recommendation system. Key techniques included handling missing values and transforming categorical variables into numeric representations.

By addressing missing values, potential biases and inaccuracies from incomplete data were mitigated. Various approaches such as imputation or deletion were employed based on the nature of the missing values and the project requirements. Additionally, transforming categorical variables into numeric forms was essential for their inclusion in machine learning models. Techniques like one-hot encoding or label encoding were applied depending on the characteristics of the categorical variables.

Moreover, a vector feature was created to ensure compatibility with Spark ML models. Although this vector feature was not utilized later in the code, its creation signifies a proactive approach to preparing the data for potential model training or future iterations of the recommendation system. The vector feature is crucial for organizing the data in a format suitable for machine learning algorithms, significantly impacting the models' accuracy and performance.

## Machine Learning
The project employed collaborative filtering, specifically the Alternating Least Squares (ALS) algorithm, for training the recommendation model. Collaborative filtering is a popular approach in recommendation systems, leveraging the preferences and behavior of similar users or items to make personalized recommendations.

### What is ALS (Alternating Least Squares)?
ALS is a matrix factorization-based collaborative filtering algorithm that decomposes the user-item interaction matrix into low-rank matrices. It assumes that users' preferences can be represented by a few latent factors and learns these factors through an iterative optimization process. The ALS algorithm in Spark MLlib offers a scalable and efficient implementation of collaborative filtering, handling large datasets through distributed computing capabilities and parallel processing. During the model training phase, the algorithm optimizes the factorized matrices to minimize the reconstruction error between predicted and actual user-item interactions, capturing the latent factors that best represent users' preferences.

By leveraging collaborative filtering with ALS, the project utilized the collective wisdom of users to make recommendations. The model identifies similar users or items based on their historical interactions and infers preferences based on the behavior of like-minded users or similar items. This approach captures implicit preferences and uncovers hidden connections within the dataset, enabling the model to generate personalized recommendations. The ALS-based collaborative filtering approach provides good scalability, making it suitable for large-scale recommendation systems, and aims to enhance user experience and engagement on the e-commerce platform.

### Spark Workflow Data Pipeline 
A pipeline was used to streamline the process of training the ALS model and making predictions on the testing set. A pipeline in Spark ML is a sequence of stages executed in a specified order, with each stage representing a distinct data transformation or machine learning operation. The purpose of using a pipeline in this code is to encapsulate and automate the entire workflow, from data preparation to model training and prediction generation. By defining a pipeline, all necessary transformations and operations are organized into a coherent sequence, ensuring consistency and reproducibility.

 ## Flask-Based Web Application
 In developing the Flask-based web application, functionality and user experience were enhanced by integrating JavaScript (JS) components. A key feature of the application is the Login Page, where users provide a valid email address and password for authentication, ensuring secure access to personalized features.

A strategy was implemented to handle cases where users enter a reviewerID new to the prediction model. Recognizing that collaborative filtering models like ALS rely on past user experiences for accurate recommendations, we accounted for scenarios with no historical data. In such cases, users are redirected to a page displaying the top 5 generic ASINs, offering initial recommendations based on popular or frequently chosen products. This approach provides valuable suggestions even in the absence of personalized historical data, enhancing user experience. As users engage more with the platform, the collaborative filtering model progressively incorporates their preferences to offer more accurate and tailored recommendations over time.

## Live Streaming
Apache Kafka was successfully integrated into the web-based application to generate live user data.

### Consumer Setup for Real-time Streaming:

The Kafka Consumer configuration and setup involve specifying bootstrap servers, group ID, and offset reset to receive messages from the specified topic ('test'). The Consumer subscribes to the topic and continuously polls for new messages. Upon receiving a message without errors, it is processed and yielded as a real-time event in the event_stream function.


### Event Stream Function and Streaming Route:
The event_stream function, defined within the '/stream' route of the Flask web application, utilizes an infinite loop to continuously receive messages from the Kafka Consumer. Received messages are processed and transformed into a streamable format, then yielded to the web client as part of a Server-Sent Events (SSE) response, enabling the web application to stream real-time recommendations based on user preferences. 

### Kafka Producer Setup and Sending Recommendations:
The Kafka Producer setup and message delivery involve initializing the Producer with bootstrap servers configuration. Within the '/result' route, which handles user interactions and recommendation generation, the top 5 ASINs (Amazon Standard Identification Numbers) are obtained, transformed into a list format suitable for JSON serialization, and sent to the Kafka topic ('test') using the produce function. Upon successful delivery, the delivery_report function handles the delivery status of the message.



