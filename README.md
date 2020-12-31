# DApp-project
Repository for the project of Distributed Applications in the Edge-Cloud Continuum at the University of Innsbruck: 
https://github.com/sashkoristov/DAppMaster-2020W


# Project 3: Prediction of stock prices `Storage, Forecast, webhook` (Python, Java, node.js)

You'll create a real-life FC to predict the price of stocks you can buy and sell.

## Introduction

### Motivation

You frequently trade with a wide variety of stocks on various stock exchanges. Every morning, your assistant compiles you a list of 50 stock that have performed interestingly and might be worth a look.

You want to see at a glance how these stocks could perform. Therefore you program an FC that takes these names, predicts their prices, and visualises their past and projected price.

This should be done in parallel, per stock. The result should be visualised together. The FC shoud return an URL of the visualisation.


### Input

```
['AAL', 'ALT', 'NCMI', ... ]
```

* A list of stock ticker symbols traded on some exchange.



### Rough steps

* Pull commodity prices to the storage
* Forecast for the coming year for each commodity
* Create a chart showing the past and future price of all commodities.


## MS2 (17.12.2020): Analyze the given FC with AFCL


The given FC receives a single collection stockTickers and the folder of a storage where to store the pulled data.


## MS3 (07.01.2021, Homework 06): Code the functions + deploy them on across ultiple regions with a Terraform script

Details will be given with Homework 06.

### Functions

#### Fetch and process (`fetchProcess`)

This function should pull the historical daily prices of each stock in parallel to the storage. You can use [AlphaVantage](https://www.alphavantage.co/) to retrieve time series of prices.
You may have to do some processing, such as stripping unnecessary fields. It is up to you what exactly you want to predict, what pre-processing or enriching you do, how you pick related data, and so on.

Hints:
* If you want to incorporate market sentiment at that time, [this may help](https://raw.githubusercontent.com/qngapparat/sentim/master/python/qmarketin500.csv).

The function gets the input from the FC and returns a collection of prices (locations on the storage) and how many are they. 

After this function, the parallelFor pTicker starts for each stock ticker.

Optional: 
* In case you download huge amount of data, you may want to distribute the data across storages in multiple regions so that other functions access data from the storage in the corresponding region. In such case, see the FC of project 1 (folderFrames).

#### Forecast (`Forecast`)

Start the forecast for given commidity and move the results to a file on the storage. 

The function gets a location of the prices and their Ticker and returns the link where the predicted prices are stored. Since we run in parallel, each instance will store a single file with predicted prices for the given Ticker.

<!--Hints:
* Since you will be generating JavaScript code, it's useful to code this function in NodeJS
-->



#### Process result (`processResult`)

Fetch the result file for the given commodity from the storage and prepare it for visualisation. Strip fields that you aren't interested in. Save it in a way that's easy to read for the following step.

This function gets a link to the file with predicted prices and returns another link (processed predicted prices).

#### Create chart (`createChart`)

This function gets the list of processed predicted prices (locations) and returns a collection of locations for charts.

Fetch all the result files, and create one or more charts that visualizes the past and projected price of the stocks.
Again, you have plenty leeway what you want to do here.


Hints:
* For creating charts the [Quickchart API](https://quickchart.io/) is convenient.


## MS4 (21.01.2021, Homework 07)

Develop the given scheduler to schedule the FC across all function deployments.
Based on the output of the scheduler, build a CFCL file by adapting the parallelFor as you learned in Homework 04. 

Details will be given with Homework 07.

