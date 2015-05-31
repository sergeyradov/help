The Apdex Alliance is a group of companies collaborating to promote an application performance metric called Application Performance Index (Apdex). For easier comparison of response time between Flood tests we show an Apdex score in your Floods dashboard and individual results.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-08_13-25-23.jpg)

The fundamental objective of Apdex is to simplify the reporting of application response time measurements by making it possible to represent any such measurement using a common metric.

Response time data can describe a wide range of targets, and its magnitude can vary widely. The index is designed to normalize for this variability of time (wide range of seconds) and measurement requirements (many distinct targets), producing a single metric that always has the same meaning.

Users have a finite set of reactions or views by which they characterize application response time. Each such group of time durations is called a performance zone. Performance zones are defined by two thresholds – times where the zone begins and ends. Apdex defines three such performance zones:

__Satisfied__ Response times that are fast enough to satisfy the user, who is therefore able to concentrate fully on the work at hand, with minimal negative impact on his/her thought process.

__Tolerating__ Responses in the tolerating zone are longer than those of the satisfied zone, exceeding the threshold at which the user notices how long it takes to interact with the system, and potentially impairing the user’s productivity. Application responses in this zone are less than ideal but don’t by themselves threaten the usability of the application.

__Frustrated__ As response times increase, at some threshold the user becomes unhappy with slow performance entering the frustrated zone. With response times in the frustrated zone, a casual user is likely to abandon a course of action and a production user is likely to cancel a Task.

The three zones are defined by two thresholds: T and F, in seconds, as follows.

- Satisfied Zone = zero to T.
- Tolerating Zone = Greater than T to F.
- Frustrated Zone = Greater than F.

An Apdex score is then calculated based on the following formula

![](https://s3.amazonaws.com/flood-io-support/apdex.orgdocumentsApdexTechnicalSpecificationV11_000.pdf_2014-10-08_13-18-22.jpg)

A qualitative result can then be produced as follows:

0.94 to 1.00 Excellent
0.85 to 0.93 Good
0.70 to 0.84 Fair
0.50 to 0.69 Poor
0.00 to 0.49 Unacceptable

You have the option of adding this series in any of your Flood reports

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-08_13-21-01.jpg)

You can also modify the zone thresholds in your [account settings](/dashboard/settings). All thresholds in Flood IO are based in milliseconds.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-08_20-24-51.jpg)