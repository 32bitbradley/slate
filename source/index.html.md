---
title: HoneypotDB - Beta API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='#'>Want more requests? Get an API key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Using the API

> All API responses follow a standard JSON format.

```json
{
    "data": {},
    "error": 0,
    "message": "Pong!",
    "meta": {
        "items": 0
    },
    "success": true
}
```

Welcome to HoneypotDB's beta API! You can use this API to query and interface with HoneypotDB, it's the best way to ingrate HoneypotDB with your existing infrastructure.

<aside class="warning">
HTML scrapers will be shunned, and placed on the bad IPs list 😲
</aside>

As the API is in beta, and currently under heavy development and things may be subject to change, however this documentation will be updated promptly.

Examples are shown in shell using `curl` and the `jq` package, and Python using the `requests` module.

### Authentication

The API is currently in beta and as a result requires no authentication to access, so grab it while you can! 😁

Once HoneypotDB API v1 is released, the API will still be available for free for a limited about of requests per day.

You will be able to create an account to acquire an API key, granting a higher request limit and access member only endpoints.

### Rate limiting

As much as I'd like to `tail -f /var/logs/nginx/access_log` and just watch the requests fly in, servers are expensive.

As a result, the API will be globally rate limited **during the beta period**.

<aside class="notice">
You will be rate limited to 10 requests a minute based off your IP address.
</aside>










# Ping!

## Ping!

```python
import requests

response = requests.get("https://honeypotdb.com/api/ping")

print(response.json())
```

```shell
curl "https://honeypotdb.com/api/ping" | jq .
```

> Response:

```json
{
    "data": {
        "ping": "Pong!",
        "pong": "Ping!"
    },
    "error": 0,
    "message": "Pong!",
    "meta": {
        "items": 2
    },
    "success": true
}
```

Ping!

### HTTP Request

`GET https://honeypotdb.com/api/ping`

### Query Parameters

This request takes no parameters.










# Lists

## Get All list types

```python
import requests

response = requests.get("https://honeypotdb.com/api/lists")

print(response.json())
```

```shell
curl "https://honeypotdb.com/api/lists" | jq .
```

> Response:

```json
{
    "data": {
        "lists": [
            "passwords",
            "usernames",
            "ips"
        ]
    },
    "error": 0,
    "message": "Successfully returned all list types.",
    "meta": {
        "items": 1
    },
    "success": true
}
```

This endpoint will return all available list types.

### HTTP Request

`GET https://honeypotdb.com/api/lists`

### Query Parameters

This request takes no parameters.




## Get a list

```python
import requests

response = requests.get("https://honeypotdb.com/api/lists/ips")

print(response.json())
```

```shell
curl "https://honeypotdb.com/api/lists/ips"
```

> Response:

```json
{
    "data": {
        "type": "ips"
    },
    "error": 1001,
    "message": "The ips list for today is not ready yet, please try again soon.",
    "meta": {
        "items": 1
    },
    "success": true
}
```
```json
{
    "data": {
        "from_datetime": "2020-09-17T19:09.000Z",
        "generation_success": true,
        "generation_time": "2020-09-18T19:09.000Z",
        "ips": [
            "51.91.250.49",
            "122.166.227.27",
            "103.4.217.139",
            "51.210.151.242",
            "51.75.16.138"
        ],
        "meta": {
            "items": 5
        },
        "to_datetime": "2020-09-18T19:09.000Z",
        "type": "ips"
    },
    "error": 0,
    "message": "List returned successfully.",
    "meta": {
        "items": 7
    },
    "success": true
}
```
This endpoint will return the requested list type.

If you're the first person (or bot 👋) to request that list today, you'll kick off the list generation and receive a message saying that the list is not ready yet and error code `1001`. Please wait 30 seconds and try again.

<aside class="notice">
You can replace `ips`` in the shown examples' URL with any list type returned by the `GET /api/lists` endpoint
</aside>

### HTTP Request

`GET https://honeypotdb.com/api/lists`

### Query Parameters

This request takes no parameters.

### Limits

Each list will contain a maximum of 100,000 items.











# Search

## Search for events

```python
import requests

data = {'ip': '104.131.1.89', 'minutes': '15', 'size': '5'}

response = requests.get("https://honeypotdb.com/api/search", params=data)

print(response.json())
```

```shell
curl "https://honeypotdb.com/api/search?ip=180.166.192.669&minutes=15&size=5" | jq .
```

> Response:

```json
{
    "data": {
        "hits": [
            {
                "GeoLocation": {
                    "city_name": "Clifton",
                    "country_iso_code": "US"
                },
                "ip": "104.131.1.89",
                "message": "Remote SSH version: b'SSH-2.0-libssh-0.6.3'",
                "session": "21c1a36d2896",
                "timestamp": "2020-09-20T17:59:54.440Z",
                "type": "cowrie.client.version",
                "uuid": "26912c1d-38b2-4ac7-a39f-d6930e5d017b"
            },
            {
                "GeoLocation": {
                    "city_name": "Clifton",
                    "country_iso_code": "US"
                },
                "ip": "104.131.1.89",
                "message": "New connection: 104.131.1.89:39696 (172.17.0.2:2222) [session: c06dea09c27b]",
                "session": "c06dea09c27b",
                "timestamp": "2020-09-20T17:53:05.928Z",
                "type": "cowrie.session.connect",
                "uuid": "5ad6d815-6f54-44e7-908e-bb4bd473af59"
            },
            {
                "GeoLocation": {
                    "city_name": "Clifton",
                    "country_iso_code": "US"
                },
                "ip": "104.131.1.89",
                "message": "SSH client hassh fingerprint: 51cba57125523ce4b9db67714a90bf6e",
                "session": "c06dea09c27b",
                "timestamp": "2020-09-20T17:53:05.928Z",
                "type": "cowrie.client.kex",
                "uuid": "e28706da-665f-40d2-905a-9023f6beb2f3"
            },
            {
                "GeoLocation": {
                    "city_name": "Clifton",
                    "country_iso_code": "US"
                },
                "ip": "104.131.1.89",
                "message": "login attempt [root/ucloud.cn] failed",
                "password": "ucloud.cn",
                "session": "c06dea09c27b",
                "timestamp": "2020-09-20T17:53:05.928Z",
                "type": "cowrie.login.failed",
                "username": "root",
                "uuid": "ba3aab10-10a2-4814-8745-fc5a2119f782"
            },
            {
                "GeoLocation": {
                    "city_name": "Clifton",
                    "country_iso_code": "US"
                },
                "ip": "104.131.1.89",
                "message": "Remote SSH version: b'SSH-2.0-libssh-0.6.3'",
                "session": "c06dea09c27b",
                "timestamp": "2020-09-20T17:53:05.928Z",
                "type": "cowrie.client.version",
                "uuid": "a32853b4-e187-4902-b07d-8a07a47ac88f"
            }
        ],
        "meta": {
            "from_datetime": "2020-09-20T17:47:19.000Z",
            "generation_time": "2020-09-20T18:02:19.000Z",
            "items": 5,
            "query": "ip=104.131.1.89&minutes=15&size=5",
            "size": 5,
            "to_datetime": "2020-09-20T18:02:19.000Z"
        }
    },
    "error": 0,
    "message": "Search completed successfully.",
    "meta": {
        "items": 2
    },
    "success": true
}
```

Use this endpoint to search HoneypotDB, any combination of search parameters and be used. 

The datetime to search from (from_time) can be specified by specifying any combination of datetime parameters. Alternatively, a specific from data can be specified in the specified format. This from_time can be any date up to 1 week ago.

You may also specify a specific to datetime, again in the specified format to search to (to_datetime).

The size parameter can be provided to limit the amount of returned results to the first X returned from HoneypotDB's database. A default of 100 is set if no size parameter is provided.

### HTTP Request

`GET https://honeypotdb.com/api/search`

### Query Parameters

Any combination of the search parameters listed below can be provided, the returned results will match all the provided parameters.

Additionally, any combination of datetime parameters listed below can be provided to calculate the search from time, excluding the parameters `from_time` and `to_time`, which is used to explicitly set these to a provided datetime.

**Datetime parameters**

| Parameter | Default | Required | Format             | Description                                                                       |
| --------- | ------- | -------- | ------------------ | --------------------------------------------------------------------------------- |
| seconds   | 0       | No       | Any integer        | The time in seconds used to calculate the from_datetime                          |
| minutes   | 0 *     | No       | Any integer        | The time in seconds used to calculate the from_datetime                          |
| hours     | 0       | No       | Any integer        | The time in hours used to calculate the from_datetime                            |
| days      | 0       | No       | Any integer        | The time in days used to calculate the from_datetime                             |
| weeks     | 0       | No       | Any integer        | The time in weeks used to calculate the from_datetime                            |
| from_time | 0       | No       | YYY-MM-DDTHH:MM:SS | An explicit datetime to use as the from_datetime, for example 2020-09-20T12:45:20 |
| to_time   | 0       | No       | YYY-MM-DDTHH:MM:SS | An explicit datetime to use as the from_datetime, for example 2020-09-20T12:45:20 |

\* If no datetime parameters are specified, the default of 15 minutes will be applied.

*All dates are in UTC*

**Search parameters**

| Parameter | Default | Required | Format                    | Description                                   |
| --------- | ------- | -------- | ------------------------- | --------------------------------------------- |
| ip        | None    | No       | Any string                | The ip to include in the search query         |
| session   | None    | No       | Any string                | The session to include in the search query    |
| username  | None    | No       | Any string                | The username to include in the search query   |
| password  | None    | No       | Any string                | The password to include in the search query   |
| uuid      | None    | No       | Any string formatted uuid | The event uuid to include in the search query |

\* If no search parameters are specified, all results within the given time period are returned.

**Results parameters**

| Parameter | Default | Required | Format      | Description                                             |
| --------- | ------- | -------- | ----------- | ------------------------------------------------------- |
| size      | 100     | No       | Any integer | The first X number of results in the database to return |


### JSON Response

**Meta items**

| Parameter       | Default            | Description                                |
| --------------- | ------------------ | ------------------------------------------ |
| from_time       | None               | The from time in UTC used in the search    |
| to_time         | None               | The to time in UTC used in the search      |
| generation_time | Current tie in UTC | The datetime that your query was requested |
| size            | None               | The amount if hits returned by the search  |
| query           | None               | Your provided query string                 |

**Hit items**

| Parameter                    | Default | Description                                                                         |
| ---------------------------- | ------- | ----------------------------------------------------------------------------------- |
| ip                           | None    | The source IP of the event, if included in the DB                                   |
| message                      | None    | The message containing extra information regarding the event, if included in the DB |
| session                      | None    | The session id of the event, if included in the DB                                  |
| timestamp                    | None    | The timestamp of the DB                                                             |
| type                         | None    | The type of event, if included in the DB                                            |
| uuid                         | None    | The event's uuid (Unique universal ID)                                              |
| GeoLocation.city_name        | None    | The source IP's GeoLocation city name, if included in the DB                        |
| GeoLocation.country_iso_code | None    | The source IP's GeoLocation country ISO code, if included in the DB                 |