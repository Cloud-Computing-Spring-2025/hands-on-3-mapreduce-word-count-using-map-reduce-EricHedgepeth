
# Project Overview 

This lab/ hands on assignment implements a word count program using the MapReduce framework from Hadoop. This program takes a given text file and outputs the amount of times each word is used in the dataset. The lab containerized using docker to simulate using a hadoop cluster environment.  

# Approach and Implementation

* Mapper
    -The Mapper reads the input file line by line, breaks each line into individual words, and removes any punctuation. For every word it finds, it creates a (word, 1) pair

* Reducer 
    -The Reducer takes all those word counts and adds them up. If the Mapper found "Hadoop" five times in different places, the Reducer will sum those 1s together and output the total times the word was reportly found. 

# Execution Steps 

* docker compose up -d 

* mvn install

* mv target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar shared-folder/input/code/

* docker cp shared-folder/input/code/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/

* docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/

* docker exec -it resourcemanager /bin/bash

* hadoop fs -mkdir -p /input/dataset

* hadoop fs -put /opt/hadoop-3.2.1/share/hadoop/mapreduce/input.txt /input/dataset

* hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/dataset/input.txt /output

* hadoop fs -cat /output/*

* hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/

* exit

* docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/

# Challenges Faced & Solutions 

* I faced several challeneges. I had a hard time initially because my VScode on my mac was quarantined and I had to look up how to fix that. It also has been years since I used Java so I had to look up serveral syntax and libraries to understand what I was doing. I went home and switched to my windows because I was encountering issues with some of the commands and a utf-8 error.  

# Sample Input and Output

---
**Sample Input **
---


    Hello world
    Hello Hadoop
    MapReduce is awesome
    Hadoop is great

---
**Sample Output **
---

    Hadoop   2
    Hello    2
    MapReduce 1
    awesome  1
    great    1
    is       2
    world    1