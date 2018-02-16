---
layout: post
title: "Downloading Kaggle Competition Data"
author: "KBSH"
tags: ["Kaggle", "Data Science", "Kaggle Competetion data", "Command line download", "kaggle command line download"]
categories: journal
---

### What is Kaggle?
Kaggle is the leading Data Science Competetion platform where data scientists, data analysts, and machine learning scientists compete for generating the best models on the data sets uploaded by different private and public entities. 
* Other competetors include
    * Driven Data
    * Analytics Vidya
    * Crowd AnalytiX

### Downloading the Kaggle Competetion Data?
Due to the high volumes of the competetion data, downloading the kaggle competetion data can be cumbersome, and also there are some cases, where we need to download the data directly on the remote servers / clouds. Initial way is to download the data on to you local machine and then scp to the remote server or to the cloud. But there is more efficient way in achieving this through the [Kaggle-cli](https://github.com/floydwch/kaggle-cli) (an unofficial kaggle command line tool)

* Downloading kaggle-cli

```
$ sudo pip install kaggle-cli
```
Once the kaggle-cli is installed on the server, this will expose kg shell command, using which we can the data for a particular competetion and even submit the prediction results. 

* Downloading the data for a competetion

```
$ kg download -u <username> -p <passwrd> -c <competetion>
```

This will download all the files related to that comptetion to your current directory. 

* Downloading a specific data file in the competetion

```
$ kg download -u <username> -p <passwrd> -c <competetion> -f <filename>
```
        
Note: Before downloading the data for any competition make sure you are entering the competetion by accepting the terms and conditions of the competetion, then only you can download the data for that particular comptetion. 

* Making submission for a competition

```
$ kg submit <filename> -u <username> -p <passwrd> -c <competetion> -m "message"
```
* Example: 

```
$ kg download -u kbsriharsha@gmail.com -p harsha124 -c datasciencebowl2018 
```
```
$ kg submit submission1.csv kbsriharsha@gmail.com -p harsha124 -c datasciencebowl2018 -m "TrailSubmission"
```
* Other Important commands include
    * complete - print bash completion command
    * config   - Set config
    * dataset  - Download dataset from a specific user
    * download - Download data files from a specific competition
    * help     - print detailed help for another command
    * submissions - List recent submissions
    * submit   - Submit an entry to a specific competition

