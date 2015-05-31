Here are some ways to troubleshoot a problem flood. If you are still stuck or need assistance please [contact support](mailto:support@flood.io).

### Check STDOUT and STDERR logs on your grid for any errors.

Your grid logs might reveal the source of the error, particularly any script compilation issues.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2015-03-01_19-58-37.jpg)

### Access raw logs on your grid

You can access all the [raw logs](https://flood.io/help/floods/accessing_raw_logs) on the grid you last ran the flood test on until you start the next test, or the grid is terminated. Flood IO does not store these long term. Alternatively you can archive these logs so you can download at your own leisure.

### Write to a custom log

You can use a custom JMeter listener such as `Save Responses to a File` to write to `/var/log/flood/custom/` which can then be accessed from your grids remote files.

![](https://s3.amazonaws.com/flood-io-support/problem_flood.jmx_UserstimkoopmansDesktopproblem_flood.jmx_-_Apache_JMeter_2.12_r1636949_2015-03-01_20-19-51.jpg)