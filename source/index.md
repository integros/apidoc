---
title: Integros API Reference

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
curl "https://api.integros.com/v1/widgets/uploader.js?access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

```shell
curl "https://api.integros.com/v1/widgets/uploader.js" \
     -H 'Authorization: Token token="7A939A92420E4C9BF075976ED5E82BA9"'
```

# Uploading widget

```html
<script>
  var _integros = _integros || [];

  _integros.push(['_setApiUrl', 'https://api.integros.com/v1/uploads/get_resumable_url']);
  _integros.push(['_setAccessKey', '{{ access_key }}']);
  _integros.push(['_setStylesUrl', 'https://api.integros.com/uploading_widget/uploading_widget.css']);
  _integros.push(['_setContainerId', '#uploading-widget']);
  _integros.push(['_setOnFileProgress', function() { console.log('Progress', arguments); }]);
  _integros.push(['_setOnFileSuccess', function() { console.log('Success', arguments); }]);
  _integros.push(['_setOnFileError', function() { console.log('Error', arguments); }]);
  _integros.push(['_setOnBeforeAdd', function() { console.log('BeforeAdd', arguments); return true; }]);
  _integros.push(['_setLanguage', 'eng']);
  _integros.push(['_setSingleFile', true]);
  _integros.push(['_setFileName', 'custom_file_name.mp4']);

  (function() {
    var script = document.createElement('script');
    script.type = 'text/javascript';
    script.async = true;
    script.src = '//api.integros.com/uploading_widget/uploading_widget.js';
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(script, s);
  })();
</script>

<div id="uploading-widget" class="ivs-uploading-widget"></div>
```

Parameter           | Default                                                 | Description
------------------- | ------------------------------------------------------- | -----------
_setApiUrl          | https://api.integros.com /v1/uploads/get_resumable_url   |
_setAccessKey       |                                                         |
_setStylesUrl       | https://api.integros.com /uploading_widget/uploading_widget.css | Custom styles url
_setContainerId     |                                                         |
_setOnFileProgress  |                                                         | Callback
_setOnFileSuccess   |                                                         | Callback
_setOnFileError     |                                                         | Callback
_setOnBeforeAdd     |                                                         | Validation callback. If it's set need to return `true` or `false`.
_setLanguage        | rus                                                     | `rus` or `eng`
_setSingleFile      | false                                                   | Single file uploader?
_setFileName        |                                                         | File name for single upload


> or you can use it manually

```js
window.uploader = new Integros.Uploader({
  apiUrl: 'http://api.integros.dev/v1/uploads/get_resumable_url',
  apiAccessKey: '7A939A92420E4C9BF075976ED5E82BA9',
  singleFile: true,
  fileName: 'custom_file_name.mp4',
  widget: {
    container: '#uploading-widget'
  }
})
```

# Uploading

## Get url for [ResumableJS](http://www.resumablejs.com/)

```shell
curl "https://api.integros.com/v1/uploads/get_resumable_url.js?access_key=7A939A92420E4C9BF075976ED5E82BA9" -I
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

`GET https://api.integros.com/v1/uploads/get_resumable_url.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
files     | false   | Array of Hash <br/> `{ "id": "id1", "name": "file1.mp4", "size": 12312323 }`

## Get url for uploading single file

```shell
curl "https://api.integros.com/v1/uploads/get_url.json?access_key=7A939A92420E4C9BF075976ED5E82BA9"
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

`GET https://api.integros.com/v1/uploads/get_url.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
file_name | false   | Video file name


## Upload video by url

```shell
curl "https://api.integros.com/v1/uploads/upload_url.json?file_url=http://media.filmz.ru/trailer_rus/i/interstellar_trailer_rus.mp4&access_key=7A939A92420E4C9BF075976ED5E82BA9"
```

> The above command returns JSON structured like this:

```json
{ "token": "461c16e7e9fa7656" }
```

### HTTP Request

`GET https://api.integros.com/v1/uploads/upload_url.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
file_url  | false   | Video URL
file_name | Fetch from `file_url`    | Video file name


# Videos

## Get list of videos

```shell
curl "https://api.integros.com/v1/videos.json?access_key=7A939A92420E4C9BF075976ED5E82BA9"
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
    "original_filename": "Dixar.For.theS01E05.x264-Black$caR.mkv",
    "filesize": 11305838
  },
  {
    "id": 21,
    "token": "41006f91e122ea15",
    "state": "done",
    "progress": 100,
    "created_at": "2014-06-19T17:32:54.556+04:00",
    "updated_at": "2014-06-19T17:36:46.526+04:00",
    "original_filename": "Pixar.For.the.Birds.2001.x264-Black$caR.mkv",
    "filesize": 11305838
  }
]
```

### HTTP Request

`GET https://api.integros.com/v1/videos.json`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
page      | 1       | Page number
per_page  | 30      | Videos per page


## Get video detail information

```shell
curl "https://api.integros.com/v1/videos/b84b5c8571243115.json?access_key=7A939A92420E4C9BF075976ED5E82BA9"
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
  "original_filename": "Dixar.For.theS01E05.x264-Black$caR.mkv",
  "filesize": 11305838
}

```

### HTTP Request

`GET https://api.integros.com/v1/videos/:id.json`
