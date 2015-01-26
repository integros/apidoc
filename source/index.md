---
title: API Reference

language_tabs:
  - shell
  - ruby

toc_footers:
  - <a href='https://ivs.integros.com'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Integros Video Layer API! You can use our API to access Integros Video Layer API endpoints, which can create task for uploading videos and get information about current video status.

# Authentication

To authorize, use `access_key` parameter, which can be pass in url, or you can pass token in HTTP headers.

```shell
curl "http://api.integros.dev/v1/widgets/uploader.js?access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

```shell
curl "http://api.integros.dev/v1/widgets/uploader.js" \
     -H 'Authorization: Token token="7A939A92420E4C9BF075976ED5E82BA9"'
```

# Uploading

## Get url for [ResumableJS](http://www.resumablejs.com/)

```shell
curl "http://api.integros.dev/v1/uploads/get_resumable_url.js?access_key=7A939A92420E4C9BF075976ED5E82BA9" -I
```

> The above command returns JSON structured like this:

```json
{
  "id1": {
    "token": "461c16e7e9fa7656",
    "url": "http://ivs.integros.com:8081/upload_chunk?token=461c16e7e9fa7656&expires_at=12312321&sign=h47hf84ufh484ufh"
  }
}
```

### HTTP Request

`GET http://api.integros.com/v1/uploads/get_resumable_url.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
files     | false   | Array of Hash <br/> `{ "id": "id1", "name": "file1.mp4", "size": 12312323 }`

## Get url for uploading single file

```shell
curl "http://api.integros.com/v1/uploads/get_url.json?access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

> The above command returns JSON structured like this:

```json
{
  "token": "461c16e7e9fa7656",
  "url": "http://ivs.integros.com:8081/upload_chunk?token=461c16e7e9fa7656&expires_at=12312321&sign=h47hf84ufh484ufh"
}
```

```shell
curl -i -F token=461c16e7e9fa1951 -F sign=e5a3997da9ddb73b4bed8e981a30bfb53164333da -F expires_at=1417181218 -F file=@GOPR1012.MP4 http://api.integros.com:8081/upload
```

### HTTP Request

`GET http://api.integros.com/v1/uploads/get_url.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
file_name | false   | Video file name
file_size | null    | Video filesize


## Upload video by url

```shell
curl "http://api.integros.com/v1/uploads/upload_url.json?file_url=http://media.filmz.ru/trailer_rus/i/interstellar_trailer_rus.mp4&access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

> The above command returns JSON structured like this:

```json
{ "token": "461c16e7e9fa7656" }
```

### HTTP Request

`GET http://api.integros.com/v1/uploads/upload_url.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
file_url  | false   | Video URL
file_name | Fetch from `file_url`    | Video file name


# Videos

## Get list of videos

```shell
curl "http://api.integros.com/v1/videos.json?access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 20,
    "token": "505eea833221d410",
    "state": "done",
    "progress": 100,
    "created_at": "2014-06-19T17:32:41.153+04:00",
    "updated_at": "2014-06-19T17:34:56.436+04:00",
    "original_filename": "Dixar.For.theS01E05.x264-Black$caR.mkv"
  },
  {
    "id": 21,
    "token": "41006f91e122ea15",
    "state": "done",
    "progress": 100,
    "created_at": "2014-06-19T17:32:54.556+04:00",
    "updated_at": "2014-06-19T17:36:46.526+04:00",
    "original_filename": "Pixar.For.the.Birds.2001.x264-Black$caR.mkv"
  }
]
```

### HTTP Request

`GET http://api.integros.com/v1/videos.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
page      | 1       | Page number
per_page  | 30      | Videos per page


## Get video detail information

```shell
curl "http://api.integros.com/v1/videos/b84b5c8571243115.json?access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

> The above command returns JSON structured like this:

```json
{
  "id": 20,
  "token": "505eea833221d410",
  "state": "done",
  "progress": 100,
  "created_at": "2014-06-19T17:32:41.153+04:00",
  "updated_at": "2014-06-19T17:34:56.436+04:00",
  "original_filename": "Dixar.For.theS01E05.x264-Black$caR.mkv"
}

```

### HTTP Request

`GET http://api.integros.com/v1/videos/:id.json`
