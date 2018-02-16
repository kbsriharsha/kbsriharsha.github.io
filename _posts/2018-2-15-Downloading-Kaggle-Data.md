---
layout: post
title: "Downloading Kaggle Competition Data"
author: "KBSH"
categories: journal
---

### What is Kaggle?
Kaggle is a leading Data Science Competetion platform where data scientists, data analysts, and machine learning scientists compete for generating the best models on the data sets uploaded by different private and public entities. 
* Other competetors include
    * Driven Data
    * Analytics Vidya
    * Crowd AnalytiX

### Downloading the Kaggle Competetion Data?
Due to the high volumes of the competetion data, downloading the kaggle comptetion data can be cumbersome, and also there are some cases, where we need to download the data directly on the remote servers / clouds. Initial way is to download the data on to you local machine and then scp to the remote server or to the cloud. But there is more efficient way in achieving this through the [Kaggle-cli](https://github.com/floydwch/kaggle-cli) (an unofficial kaggle command line tool)

* Downloading kaggle-cli

    *sudo pip install kaggle-cli*

Once the kaggle-cli is installed on the server, this will expose kg shell command, using which we can the data for a particular competetion and even submit the prediction results. 

* Downloading the data for a particular competetion

*kg download -u ******@gmail.com' -p '*******' -c '*******************'*
      * u - username
      * p - password
      * c - competetion name
Note: 


