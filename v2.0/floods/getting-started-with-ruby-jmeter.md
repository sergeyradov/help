# Getting Ruby-JMeter

Tired of using the JMeter GUI or looking at hairy XML files?

This [gem](https://github.com/flood-io/ruby-jmeter) lets you write test plans for JMeter in your favourite text editor, and optionally run them on Flood IO.

Install it yourself as:

```
$ gem install ruby-jmeter
```

# Creating a Test Plan

Once you have created or downloaded our [basic test plan](https://flood.io/ruby-jmeter-example.rb) and saved it locally as a `.rb` file, you can use the `.jmx` method to run locally or th `.flood` method to launch this on Flood IO.

To do so, you require an account and API token. If you don't know your token, sign in to Flood IO and check your account settings.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-07_08-08-22.jpg)

To execute the test on Flood IO, call the `.flood` method on the test and pass it the API token like this.

```
test do
  threads count: 10 do
    visit name: 'Google Search', url: 'http://google.com'
  end
end.flood('xmzyqayX5dsjsVLqGW1q')
```

This will then provide you with a link to the live test results on flood.io like this.

`Results at: https://flood.io/d384673f64e3a3`

# More Examples

For a more detailed examples with Ruby-JMeter find out [how to test a Ruby on Rails application](https://blog.flood.io/how-to-test-a-ruby-on-rails-application-with-jmeter) or [how to test a RESTful API](https://blog.flood.io/load-testing-a-restful-api-with-ruby-jmeter).