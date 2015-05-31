To have Flood IO automatically repeat a test when you commit or submit a pull request, you need to create a webhook using the [Flood API](https://flood.io/blog/6-flood-api). You can repeat existing flood tests with the following endpoint:

`POST api.flood.io/floods/:flood_id/webhook`

Webhooks allow external services to be notified when certain events happen on GitHub. When the specified events happen, GitHub will send a POST request to each of the URLs you provide.

![](https://s3.amazonaws.com/flood-io-support/Add_webhook_20140505_132116.png)

#### Parameters

- `region` *optional* target region to launch the test in
- `grid` *optional* use a specific grid UUID to launch the test in instead of a region

If a grid `region` or `uuid` is not specified then the flood test will be repeated on all available grids for your account.

#### Example

```
curl --silent -X POST https://api.flood.io/floods/K9MHO2UsoSuRKmkjIGMZqg/webhook
```

## Security

This webhook allows third party services to repeat a specific test, and allows anybody who has it to run a test against your server. If you think your webhook has been compromised you should delete the corresponding flood test from your account to prevent further use of that token.