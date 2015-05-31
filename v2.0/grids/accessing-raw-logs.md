JMeter `jtl`, Gatling `simulation`, `STDOUT` and `STDERR` logs are available on the grid you last ran the flood test on until you start the next test, or the grid is terminated. Flood IO does not store these long term.

These files are then available via your Grid dashboard:

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-07_21-44-04.jpg)

Summary results (charts/report) are stored with 15 second granularity in line with data retention period for your plan.

Archived logs (JMeter/Gatling/Custom/STDOUT/STDERR) are stored for 30 days if you enable the `Advanced -> Archive` option when starting your next flood.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-07-30_21-20-17.png)

At the completion of the flood, you will see an Archiving status for the time it takes to individually compress and sync copies of these archives to Flood IO.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-07-30_21-29-06.png)

You will then be able to download these compressed archives at your leisure. Simply use the `Actions -> Download Archived Results` link in your [floods dashboard](/dashboard/floods).

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-07_21-53-01.jpg)

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-07_21-53-59.jpg)