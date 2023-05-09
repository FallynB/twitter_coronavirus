# Coronavirus twitter analysis

All geotagged tweets sent in 2020 to monitor for the spread of the coronavirus on social media were analyzed using my program.

**Project Objectives:**

1. work with large scale datasets
1. work with multilingual text
1. use the MapReduce divide-and-conquer paradigm to create parallel code

## Background

**About the Data:**

Approximately 500 million tweets are sent everyday.
Of those tweets, about 2% are *geotagged*.
That is, the user's device includes location information about where the tweets were sent from.In total, there are about 1.1 billion tweets in this dataset.



**About MapReduce:**

By following the [MapReduce](https://en.wikipedia.org/wiki/MapReduce) procedure, I was able to analyze these tweets.MapReduce is a famous procedure for large scale parallel processing that is widely used in industry.It is a 3 step procedure summarized in the following image:

<img src=mapreduce.png width=100% />

**MapReduce Runtime:**

Let $n$ be the size of the dataset and $p$ be the number of processors used to do the computation.
The simplest and most common scenario is that the map procedure takes time $O(n)$ and the reduce procedure takes time $O(1)$.
(These will be the runtimes of our map/reduce procedures.)
In this case, the overall runtime is $O(n/p + \log p)$.
In the typical case when $p$ is much smaller than $n$,
then the runtime simplifies to $O(n/p)$.
This means that:
1. doubling the amount of data will cause the analysis to take twice as long;
1. doubling the number of processors will cause the analysis to take half as long;
1. if you want to add more data and keep the processing time the same, then you need to add a proportional number of processors.

## Programming Tasks

My procedure was as sollows:

**Task 0: Create the mapper**

I added a `map.py` file to track the usage of the hashtags on both a language and country level. Two additional variables `counter_country` and `counter_lang` were created, and I modified this variable in the `#search hashtags` section of the code appropriately.
**Task 1: Run the mapper**

I created a shell script called `run_maps.sh` to loop over each file in the dataset and run the `map.py` command on that file.Each call to `map.py` can take up to a day to finish, so you should use the `nohup` command to ensure the program continues to run after you disconnect and the `&` operator to ensure that all `map.py` commands run in parallel.

**Task 2: Reduce**
After my modified `map.py` has run on all the files,I had a large number of files in your `outputs` folder. I used the `reduce.py` file to combine all of the `.lang` files into a single file, and all of the `.country` files into a different file.

**Task 3: Visualize**
```
$ ./src/visualize.py --input_path=PATH --key=HASHTAG
```
I modified the `visualize.py` file so that it generates a bar graph of the results and stores the bar graph as a png file. The horizontal axis of the graph is the keys of the input file, and the vertical axis of the graph is the values of the input file. The final results are sorted from low to high, and you only need to include the top 10 keys.

I ran the `visualize.py` file with the `--input_path` equal to both the country and lang files created in the reduce phase, and the `--key` set to `#coronavirus` and `#코로나바이러스`.

The four plots generated are below.

![Graph 1](https://github.com/FallynB/twitter_coronavirus/blob/master/%23coronavirus_country.png)

![Graph 2](https://github.com/FallynB/twitter_coronavirus/blob/master/%23coronavirus_lang.png)

![Graph 3](https://github.com/FallynB/twitter_coronavirus/blob/master/%23%EC%BD%94%EB%A1%9C%EB%82%98%EB%B0%94%EC%9D%B4%EB%9F%AC%EC%8A%A4_country.png)

![Graph 4](https://github.com/FallynB/twitter_coronavirus/blob/master/%23%EC%BD%94%EB%A1%9C%EB%82%98%EB%B0%94%EC%9D%B4%EB%9F%AC%EC%8A%A4_lang.png)

![Alternative Reduce](https://github.com/FallynB/twitter_coronavirus/blob/master/alternativereduce.png)
