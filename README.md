# Agile Data Science 2.0 (O'Reilly, 2017)

This repository contains the updated sourcec code for [Agile Data Science 2.0](http://shop.oreilly.com/product/0636920051619.do), O'Reilly 2017. Now available at the [O'Reilly Store](http://shop.oreilly.com/product/0636920051619.do), on [Amazon](https://www.amazon.com/Agile-Data-Science-2-0-Applications/dp/1491960116) (in Paperback and Kindle) and on [O'Reilly Safari](https://www.safaribooksonline.com/library/view/agile-data-science/9781491960103/). Also available anywhere technical books are sold!

NOTE: THE BOOK'S CODE IS OLD, BUT THE CODE IS MAINTAINED. USE DOCKER COMPOSE AND THE NOTEBOOKS IM THIS REPOSITORY.

You should refer to the Jupyter Notebooks in this repository rather than the book's source code, which is badly outdated and will no longer work for you.

Have problems? Please [file an issue](https://github.com/rjurney/Agile_Data_Code_2/issues)!

## Deep Discovery

Like my work? Connect with me [on LinkedIn](https://linkedin.com/in/russelljurney)!

## Installation and Execution

There is now only ONE version of the install: Docker via the [docker-compose.yml](docker-compose.yml). It is MUCH EASIER than the old methods.

To build the `agile` Docker image, run this:

```bash
docker-compose build agile
```

To run the `agile` Docker image, defined by the [`docker-compose.yml`](docker-compose.yml) and [`Dockerfile`](Dockerfile), run:

```bash
docker-compose up -d
```

Now visit: [http://localhost:8888](http://localhost:8888)

## Other Images

To manage the `mongo` image with Mongo Express, visit: [http://localhost:8081](http://localhost:8081)

## Downloading Data

Once the server comes up, download the data and you are ready to go. First open a shell in Jupyter Lab. The working directory corresponds to this folder.

Now download the data:

```bash
./download.sh
```

## Running Examples

All scripts run from the base directory, except the web app which runs in ex. `ch08/web/`. Open [Welcome.ipynb](Welcome.ipynb) and get started.

### Jupyter Notebooks

All notebooks assume you have run the jupyter notebook command from the project root directory `Agile_Data_Code_2`. If you are using a virtual machine image (Vagrant/Virtualbox or EC2), jupyter notebook is already running. See directions on port mapping to proceed.

# The Data Value Pyramid

Originally by Pete Warden, the data value pyramid is how the book is organized and structured. We climb it as we go forward each chapter.

![Data Value Pyramid](images/climbing_the_pyramid_chapter_intro.png)

# System Architecture

The following diagrams are pulled from the book, and express the basic concepts in the system architecture. The front and back end architectures work together to make a complete predictive system.

## Front End Architecture

This diagram shows how the front end architecture works in our flight delay prediction application. The user fills out a form with some basic information in a form on a web page, which is submitted to the server. The server fills out some neccesary fields derived from those in the form like "day of year" and emits a Kafka message containing a prediction request. Spark Streaming is listening on a Kafka queue for these requests, and makes the prediction, storing the result in MongoDB. Meanwhile, the client has received a UUID in the form's response, and has been polling another endpoint every second. Once the data is available in Mongo, the client's next request picks it up. Finally, the client displays the result of the prediction to the user! 

This setup is extremely fun to setup, operate and watch. Check out chapters 7 and 8 for more information!

![Front End Architecture](images/front_end_realtime_architecture.png)

## Back End Architecture

The back end architecture diagram shows how we train a classifier model using historical data (all flights from 2015) on disk (HDFS or Amazon S3, etc.) to predict flight delays in batch in Spark. We save the model to disk when it is ready. Next, we launch Zookeeper and a Kafka queue. We use Spark Streaming to load the classifier model, and then listen for prediction requests in a Kafka queue. When a prediction request arrives, Spark Streaming makes the prediction, storing the result in MongoDB where the web application can pick it up.

This architecture is extremely powerful, and it is a huge benefit that we get to use the same code in batch and in realtime with PySpark Streaming.

![Backend Architecture](images/back_end_realtime_architecture.png)

# Screenshots

Below are some examples of parts of the application we build in this book and in this repo. Check out the book for more!

## Airline Entity Page

Each airline gets its own entity page, complete with a summary of its fleet and a description pulled from Wikipedia.

![Airline Page](images/airline_page_enriched_wikipedia.png)

## Airplane Fleet Page

We demonstrate summarizing an entity with an airplane fleet page which describes the entire fleet.

![Airplane Fleet Page](images/airplanes_page_chart_v1_v2.png)

## Flight Delay Prediction UI

We create an entire realtime predictive system with a web front-end to submit prediction requests.

![Predicting Flight Delays UI](images/predicting_flight_kafka_waiting.png)
