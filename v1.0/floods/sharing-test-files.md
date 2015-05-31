Most test plans will depend on some form of static data. Static data is any type of external data that is referenced by your load scripts. Examples include things such as post codes, user logins/passwords. Flood IO already provides a test data API which lets you [share data](https://flood.io/blog/9-sharing-test-data) amongst Grid nodes whilst maintaining uniqueness and/or state.

However if your scripts don't need this central API and you can use the same test plan / data files on each Grid node, we now offer the ability to upload multiple files for your Flood which are then distributed to each Grid node during execution.

Common use cases are things like static data stored in an external CSV, file attachments to send with a request as a multipart form request or custom scripts included or referenced by your main test plan and the like.

## Uploading Multiple Files

Whenever you create a new Flood, simply drag or click to upload multiple files to your Flood. These will then be retrieved and stored in the same path relative to the test plan itself on each Grid node during execution.

![](https://flood.io/images/blog/shared_files.png)

## Repeating Floods

You can easily repeat Floods using the same files. These will be available to you when your repeat the Flood. You can also  upload new copies of a file, just remove and add accordingly.

## Test Data APi

Most test plans will depend on some form of static data. Static data is any type of external data that is referenced by your load scripts. Examples include things such as post codes, user logins/passwords

You can upload and manage test data sets associated with a Grid. You can then access test data from your test plans via a JSON API or directly via a Redis URI.

## Uploading Data

Upload test data from your individual Grid view. Choose a CSV formatted file to upload, give the data set a memorable key name and check if you want a sorted list.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-23_14-20-21.jpg)

Sorted lists are simply lists of strings, sorted by insertion order. This will let you consume data as a queue with remove or retrieve methods for the first or last items in a queue.

If you don't choose this, then you will get a random set, which will let you remove or retrieve random items from the set.

Sets have the desirable property of not allowing repeated members. Adding the same element multiple times will result in a set having a single copy of this item.

## Accessing Data

Once test data has been uploaded you can then access test data from your test plans via a JSON API or directly via a Redis URI. Both the JSON API and Redis URI will reside on a private IP address which is only reachable from grid nodes themselves.

You can view this data via the Grid and obtain the private IP used for the API/Redis endpoint:

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-23_14-27-51.jpg)


For example, the following test plan using our [ruby-jmeter](https://github.com/flood-io/ruby-jmeter) DSL first does a HTTP GET to the test data url (hint: if you double underscore a label name, it will get excluded from Flood IO results so it doesn't affect your descriptive statistics). We then use a regular expression to extract the post code from the response. (hint: we've also specified a content type of text, instead of the default json).

We can then use the extracted response as a variable in subsequent requests, in this case we're using the value ${postcode}

```ruby
require 'ruby-jmeter'

test do
  threads 1 do
    get name: '__testdata', url: 'http://172.31.10.132/data/SRANDMEMBER/postcodes?type=text' do
      extract name: 'postcode', regex: '^(\d+)'
    end
    visit name: 'Search Post Code', url: 'http://google.com/?q=${postcode}'
  end
end.flood(ENV['FLOOD_API_TOKEN'], {region: 'us-west-2'})
```

The same test plan in JMeter looks like this:

![](https://flood.io/images/blog/uploaded_test_data_jmeter.png)

It is also possible to do the [same in Gatling](https://flood.io/blog/14-getting-started-with-gatling).

## More about the API

The API allows the following Redis like query parameters depending on the ordering of the data set uploaded, sorted or non-sorted.

### Non-sorted data

A non-sorted data set will create a Redis unordered set, which will let you retrieve a random member [SRANDMEMBER](http://redis.io/commands/srandmember) or remove a random member [SPOP](http://redis.io/commands/spop) from the data set. This is useful if you need to share random lines from the data set across multiple nodes. Example request/response to the API is shown below:

```
===> http://172.31.29.170/data/SRANDMEMBER/postcodes?type=text
<=== "3589","WOORINEN NORTH","VIC",,"WOORINEN SOUTH LPO","368","V2 ","034","VIC FAR COUNTRY","Delivery Area"

===> http://172.31.29.170/data/SPOP/postcodes?type=text
<=== "3301","STRATHKELLAR","VIC",,"HAMILTON DELIVERY","350","V2 ","034","VIC FAR COUNTRY","Delivery Area"
```

### Sorted data

A sorted data set will create a Redis ordered list, which will let you remove the first [LPOP](http://redis.io/commands/lpop) or last member [RPOP](http://redis.io/commands/lpop) from the test data set. This is useful if you need to guarantee unique lines from the data set across multiple nodes. Example request/response to the API is shown below:

```
===> http://172.31.29.170/data/LPOP/postcodes?type=text
<=== "3589","WOORINEN NORTH","VIC",,"WOORINEN SOUTH LPO","368","V2 ","034","VIC FAR COUNTRY","Delivery Area"

===> http://172.31.29.170/data/RPOP/postcodes?type=text
<=== "3301","STRATHKELLAR","VIC",,"HAMILTON DELIVERY","350","V2 ","034","VIC FAR COUNTRY","Delivery Area"
```

### Plain text response

In both cases if you use the query parameter `type=text` the response returned is a plain text line from your CSV file. You can then use standard JMeter or Gatling regular expressions to extract the desired element from that line.

### Extracting elements

It's often useful just to extract all elements with a regular expression and then use match numbers to access individual columns. For example, you can use the regular expression `"(.+?)"` to match anything in double quotes and use a match number of `-1` (in JMeter) to extract all matches. Your variable should then include the different groupings such as:

```
populate ${testdata} array with *all* results from shared data url
using default match number -1

testdata now populated with:
  testdata_1=3589
  testdata_1_g=1
  testdata_1_g0="3589"
  testdata_1_g1=3589
  testdata_2=WOORINEN NORTH
  testdata_2_g=1
  testdata_2_g0="WOORINEN NORTH"
  testdata_2_g1=WOORINEN NORTH
  ...
  testdata_9=Delivery Area
  testdata_9_g=1
  testdata_9_g0="Delivery Area"
  testdata_9_g1=Delivery Area
  testdata_matchNr=9
```

You can then just access the desired column number such as `${testdata_2}`

### Ruby-JMeter helpers

If using the Ruby-JMeter DSL we provide additional [test data helper methods](https://github.com/flood-io/ruby-jmeter/blob/master/examples/basic_testdata.rb) to abstract this and easily extract columns. It also includes the ability to stub the API when testing locally.

### Plain old CSV data sets

Don't forget, if you're happy for grid nodes to receive the same CSV you can just upload a CSV file along with your flood test and this will be made available to each node in the grid. The test data API presented in this article is more to help overcome issues such as guaranteeing uniqueness of data or sharing data from a single source across multiple grid nodes.