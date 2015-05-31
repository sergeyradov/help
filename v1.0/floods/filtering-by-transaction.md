## Filtering by Transaction

Some of our long time flooders will agree, when the number of floods you have run tallies in the thousands, searching for *that* flood test where you hit a certain URL can be painful.

Up until now, unless you have been vigilant in naming and tagging your flood tests, a rather "mandraulic" process, the process of finding those floods has been difficult.

Your [flood dashboard](/dashboard/floods) now has the ability to filter your flood tests by the request label.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2015-03-24_09-27-37.jpg)

This makes identifying which floods you hit a certain endpoint with a much more simple task.

We also present a handy time line view of flood tests with a column range (50th to 95th percentile) chart based on a logarithmic scale. This lets you identify patterns in response time performance for the transaction of interest and then select the flood test from there for more detailed analysis. Perfect for seeing the history of a particular transaction's performance over time.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2015-03-24_09-29-53.jpg)

### How it happens

To make this all happen we have been ingesting all of our cold data (previously stored on s3) back into a cluster of [Docker](https://www.docker.com/) containers running [InfluxDB](http://influxdb.com/). This aggregate layer at a resolution of 15 seconds is what we consider warm data, and is pulled into memory as needed. Our hot/live data is still sourced directly from grid nodes using [Elastic](https://www.elastic.co/). So there you have it, a distributed, loosely coupled infrastructure that stores our hot, warm and cold data..

In the near future expect to see more filtering options from your dashboard as well as more advanced analytics and reporting.