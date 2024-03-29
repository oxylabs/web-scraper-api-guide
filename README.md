# Web Scraper API Quick Start Guide

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

- [Authentication](#authentication)
- [Integration methods](#integration-methods)
  - [Push-Pull](#push-pull)
  - [Realtime](#realtime)
  - [Proxy Endpoint](#proxy-endpoint)

[Oxylabs’ Web Scraper API](https://oxy.yt/Xr2D) is a data scraper API designed to collect real-time data from websites at scale. This web scraping tool serves as a trustworthy solution for gathering information from complicated targets and ensures the ease of the crawling process. Web Scraper API best fits for cases such as website changes monitoring, fraud protection, and travel fare monitoring.

In this guide, we’ll explain how Web Scraper API works and walk you through the process of getting started with this tool without hassle. 

For a detailed explanation, see our [blog post](https://oxy.yt/rr90).

## Authentication

Web Scraper API employs basic HTTP authentication which requires username and password:

```shell
curl --user "USERNAME:PASSWORD"'https://realtime.oxylabs.io/v1/queries' -H "Content-Type: application/json" -d '{"source": "universal", "url": "https://ip.oxylabs.io/location"}'
```

## Integration methods

### Push-Pull

**Example of a single query request:**

```shell
curl --user "USERNAME:PASSWORD"'https://data.oxylabs.io/v1/queries' -H "Content-Type: application/json" -d '{"source": "universal", "url": "https://ip.oxylabs.io/location", "geo_location": "United States"}'
```
If you are observing low success rates or retrieve empty content, please try using additional `"render":"html"` parameter in your request. More information about render parameter can be found [here](https://developers.oxylabs.io/scraper-apis/getting-started/api-reference/global-parameter-values#render).

**Sample of the initial response output:**

```json
{
  "callback_url": null,
  "client_id": 1,
  "created_at": "2021-09-30 12:40:32",
  "domain": "io",
  "geo_location": "United States",
  "id": "6849322054852825089",
  "limit": 10,
  "locale": null,
  "pages": 1,
  "parse": false,
  "parser_type": null,
  "render": null,
  "url": "hhttps://ip.oxylabs.io/location",
  "query": "",
  "source": "universal",
  "start_page": 1,
  "status": "pending",
  "storage_type": null,
  "storage_url": null,
  "subdomain": "ip",
  "content_encoding": "utf-8",
  "updated_at": "2021-09-30 12:40:32",
  "user_agent_type": "desktop",
  "session_info": null,
  "statuses": [],
  "_links": [
    {
      "rel": "self",
      "href": "http://data.oxylabs.io/v1/queries/6849322054852825089",
      "method": "GET"
    },
    {
      "rel": "results",
      "href": "http://data.oxylabs.io/v1/queries/6849322054852825089/results",
      "method": "GET"
    }
  ]
}
```

In order to check whether the job is `"status": "done"`, we can use the link from `["_links"][0]["href"]` which is `http://data.oxylabs.io/v1/queries/6849322054852825089`.

**Example of how to check a job status:**

```shell
curl --user "USERNAME:PASSWORD"
'http://data.oxylabs.io/v1/queries/6849322054852825089'
```

The response will contain the same data as the initial response. If the job is `"status": "done"`, we can retrieve the contents using the link from [“_links”][1][“href”] which is `http://data.oxylabs.io/v1/queries/6849322054852825089/results`.

**Example of how to retrieve data:**

```shell
curl --user "USERNAME:PASSWORD"
'http://data.oxylabs.io/v1/queries/6849322054852825089/results'
```

**Sample of the response data output:**

```json
{
    "results": [
      {
        "content": "{\"ip\":\"45.24.127.174\",\"providers\":{\"dbip\":{\"country\":\"US\",\"asn\":\"AS7018\",\"org_name\":\"AT\\u0026T Services, Inc.\",\"city\":\"Birmingham\",\"zip_code\":\"\",\"time_zone\":\"\",\"meta\":\"\\u003ca href='https:\/\/db-ip.com'\\u003eIP Geolocation by DB-IP\\u003c\/a\\u003e\"},\"ip2location\":{\"country\":\"US\",\"asn\":\"\",\"org_name\":\"\",\"city\":\"Birmingham\",\"zip_code\":\"35201\",\"time_zone\":\"-06:00\",\"meta\":\"This site or product includes IP2Location LITE data available from \\u003ca href=\\\"https:\/\/lite.ip2location.com\\\"\\u003ehttps:\/\/lite.ip2location.com\\u003c\/a\\u003e.\"},\"ipinfo\":{\"country\":\"US\",\"asn\":\"AS7018\",\"org_name\":\"AT\\u0026T Services, Inc.\",\"city\":\"\",\"zip_code\":\"\",\"time_zone\":\"\",\"meta\":\"\\u003cp\\u003eIP address data powered by \\u003ca href=\\\"https:\/\/ipinfo.io\\\" \\u003eIPinfo\\u003c\/a\\u003e\\u003c\/p\\u003e\"},\"maxmind\":{\"country\":\"US\",\"asn\":\"AS7018\",\"org_name\":\"ATT-INTERNET4\",\"city\":\"Madison\",\"zip_code\":\"\",\"time_zone\":\"-06:00\",\"meta\":\"This product includes GeoLite2 Data created by MaxMind, available from https:\/\/www.maxmind.com.\"}}}\n", # Actual content from https://ip.oxylabs.io/location
        "created_at": "2023-12-18 10:01:59",
        "updated_at": "2023-12-18 10:01:59",
        "page": 1,
        "url": "https://ip.oxylabs.io/location",
        "job_id": "7142453937470222339",
        "status_code": 200
      }
    ]
}
```

### Realtime

With this method, you can send your request and receive data back on the same open HTTPS connection straight away. 

**Sample request:**

```shell
curl --user
"USERNAME:PASSWORD"'https://realtime.oxylabs.io/v1/queries' -H
"Content-Type: application/json" -d '{"source": "universal", "url":
"https://ip.oxylabs.io/location", "geo_location": "United States"}'
```

If you are observing low success rates or retrieve empty content, please try using additional `"render":"html"` parameter in your request. More information about render parameter can be found [here](https://developers.oxylabs.io/scraper-apis/getting-started/api-reference/global-parameter-values#render).

**Example response body that will be returned on the open connection:**

```json
{
    "results": [
      {
        "content": "{\"ip\":\"45.24.127.174\",\"providers\":{\"dbip\":{\"country\":\"US\",\"asn\":\"AS7018\",\"org_name\":\"AT\\u0026T Services, Inc.\",\"city\":\"Birmingham\",\"zip_code\":\"\",\"time_zone\":\"\",\"meta\":\"\\u003ca href='https:\/\/db-ip.com'\\u003eIP Geolocation by DB-IP\\u003c\/a\\u003e\"},\"ip2location\":{\"country\":\"US\",\"asn\":\"\",\"org_name\":\"\",\"city\":\"Birmingham\",\"zip_code\":\"35201\",\"time_zone\":\"-06:00\",\"meta\":\"This site or product includes IP2Location LITE data available from \\u003ca href=\\\"https:\/\/lite.ip2location.com\\\"\\u003ehttps:\/\/lite.ip2location.com\\u003c\/a\\u003e.\"},\"ipinfo\":{\"country\":\"US\",\"asn\":\"AS7018\",\"org_name\":\"AT\\u0026T Services, Inc.\",\"city\":\"\",\"zip_code\":\"\",\"time_zone\":\"\",\"meta\":\"\\u003cp\\u003eIP address data powered by \\u003ca href=\\\"https:\/\/ipinfo.io\\\" \\u003eIPinfo\\u003c\/a\\u003e\\u003c\/p\\u003e\"},\"maxmind\":{\"country\":\"US\",\"asn\":\"AS7018\",\"org_name\":\"ATT-INTERNET4\",\"city\":\"Madison\",\"zip_code\":\"\",\"time_zone\":\"-06:00\",\"meta\":\"This product includes GeoLite2 Data created by MaxMind, available from https:\/\/www.maxmind.com.\"}}}\n", # Actual content from https://ip.oxylabs.io/location
        "created_at": "2023-12-18 10:01:59",
        "updated_at": "2023-12-18 10:01:59",
        "page": 1,
        "url": "https://ip.oxylabs.io/location",
        "job_id": "7142453937470222339",
        "status_code": 200
      }
    ]
}
```

### Proxy Endpoint

Instead of parameters such as domain and search query, Proxy Endpoint only takes completely formed URLs. 

**Proxy Endpoint code sample in the Python programming language:**

```shell
curl -k -x realtime.oxylabs.io:60000 -U USERNAME:PASSWORD -H
"X-Oxylabs-Geo-Location: United States" "https://ip.oxylabs.io/location"
```

If you are observing low success rates or retrieve empty content, please try adding additional `"x-oxylabs-render: html"` header with your request.

If you wish to find out more about Web Scraper API Quick Start Guide, see our [blog post](https://oxy.yt/rr90).
