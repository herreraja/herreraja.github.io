---
layout:     post
title:      "Design Data-Intensive Applications: Batch Processing"
subtitle:   "Processing data using log files"
date:       2019-07-28 12:00:00
author:     "Ja.Herrera"
header-img: "img/posts/post-28072019-design-data-intensive-applications-header.png"
comments: true
tags: [ Backend, unix, data ]
---

It took me a bit longer to finish the book [Design Data-Intensive Applications](https://dataintensive.net/), not because it wasn’t good but I think it’s a reference book to consult from time to time rather than having it on the bedside table. In fact, this book is a must-have technical book for those who want to design and build systems which revolve around data.

A big part of this book talks about managing distributed systems and their common issues: replication of the nodes, how to handle leaders nodes and followers nodes when a node is down, consistency between nodes and detect failures, etc. Although we can apply some of that advice, cloud computing vendors like AWS, Google Cloud or Microsoft Azure are offering services which take care of all these concerns for us so I will focus on one of the contents of the book which it is really interesting: the data processing or batch processing.

 
## Batch Processing

A batch processing job takes a large amount of data as input, process it and produces some output data. Depending on the amount of data, it can takes minutes, hours or even days to finish all the process.

### Using simple logs analysis

What company doesn’t have a bunch of jobs doing different stuff like:

- Querying the latest transactions to export them to another system.
- Checking how many users have been registered in X period of time.
- Querying specific data for sending events to users or another service for marketing purposes like Braze.

It’s quite common to find all these jobs running on Jenkins in a big instance although some sophisticated companies run the jobs only on-demand using services like AWS Batch. This requires to dispose of an instance and query the production database which can get overloaded by connections and queries.

`We already have all this information in the logs`. Why don’t use it? Processing simple files barely require CPU (even we can use only unix tools) and we wouldn’t need to connect to the production database. We only need to be sure to `have proper logs`. Even exist services like AWS Datapipeline which automatize the log processing and do it on-demand.


```
cat /var/log/nginx/access.log | 
  awk '{print $7}' | 
  sort             | 
  uniq -c          | 
  sort -r -n       | 
  head -n 5 
```

In the example above for a nginx default access log format:

1. Read the log file.

2. Split each line into fields by whitespace, and output only the seventh such field from each line, which happens to be the requested URL. In our example line, this request URL is /css/typography.css.

3. Alphabetically sort the list of requested URLs. If some URL has been requested n times, then after sorting, the file contains the same URL repeated n times in a row.

4. The uniq command filters out repeated lines in its input by checking whether two adjacent lines are the same. The -c option tells it to also output a counter: for every distinct URL, it reports how many times that URL appeared in the input.

5. The second sort sorts by the number (-n) at the start of each line, which is the number of times the URL was requested. It then returns the results in reverse (-r) order, i.e. with the largest number first.

Finally, head outputs just the first five lines (-n 5) of input, and discards the rest. The output would look something like this:

```
4189 /favicon.ico
3631 /2013/05/24/improving-security-of-ssh-private-keys.html
2124 /2012/12/05/schema-evolution-in-avro-protocol[…]
```

An extra advantage of processing logs is to find errors in the system like failed transactions and recover them. 

![mapreduce](/img/posts/post-28072019-logs.png "MapReduce example")

During this process, we can make good use of MapReduce and Joins to obtain the desirable output.

Examples and pictures has been taken from the same book [Design Data-Intensive Applications](https://dataintensive.net/) .
