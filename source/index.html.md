---
title: My Lenio Custom App API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - code

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors


search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Custom App API

---

# Introduction

Welcome to the Custom App API! You can use our API reference to implements your endpoints, which help to integrate your app with Mylenio.

# Authentication

The API has all calls authenticated through user and password(Token). These values are included in the headers.

The first thing you need to do is get the credentials to make authenticated calls to the API. For this, in MyLenio, Access your custom application, go to the API configuration section and copy the "API User" and "APi Password" fields

> To authorize incoming request, validate these headers below:

```
   //Authentication values in header
   {
    "X-AdminUser-Email": "api.user...@mylenio.com",
    "X-AdminUser-Token" : "..."
   }
```

 `X-AdminUser-Email: user`

 `X-AdminUser-Token: token`

<aside class="notice">
You must get <code>email</code> and <code>token</code> values from the Custom App you previously created in MyLenio.
</aside>





# Basic

## API Version

The urls are generally formed as follows

`https://my.custom.api.com/api/[version]/[path]`

### Payload Parameters

Parameter |  value
--------- |  -----------
version   | <code>v1</code>
path      | <code>{request_path}</code>

## Headers

All request use application/json as content type

> Basic header to make requests:

```
   {
    ""Content-type" : "application/json" ,
    ...
   }
```


## Error Handling

See [Errors](#errors) section</a>

## Paginate Results

Paginating your results will be <code>optional</code> . In case you want to paginate your responses, you need to include a <code>links</code> field in the root of the JSON object

> A sample response with pages

```json
{
 "data": [...],
 "links":
  {
   "self":"https://my.custom.api.com/api/v1/resources?page%5Bnumber%5D=2&page%5Bsize%5D=25",
   "first":"https://my.custom.api.com/api/api/v1/resources?page%5Bnumber%5D=1&page%5Bsize%5D=25",
   "prev":"https://my.custom.api.com/api/api/v1/resources?page%5Bnumber%5D=1&page%5Bsize%5D=25",
   "next": null,
   "last":"https://my.custom.api.com/api/api/v1/resources?page%5Bnumber%5D=2&page%5Bsize%5D=25"}
  }
```

# API Status

API Status method should be a quick test to check if the API is currently live and the basic configuration parameters are setted properly

### HTTP Request

<code> GET /status </code>

> The method waits for the basic JSON object with <code>200 CODE</code> like this

```json
  {}
```

> In case of reporting an error

```json
  {
     "error" : { "code" :  "custom_code_error" , "message": "Brief description about the problem"}
  }
```

*No parameters are required*

# Users

## List users

### HTTP Request
<code> GET /users </code>

> List Users - Response body

```json
  {
    "data": [
      {
        "id": 1,
        "email": "user1@example.com",
        "name": "User 1"
      },      {
        "id": 2,
        "email": "user2@example.com",
        "name": "User 2"
      }
    ]
  }
```

*No parameters are required*

## Create a new user

### HTTP Request
<code> POST /users </code>

> Create User - Request Body

```json
      {
        "email": "user2@example.com",
        "name": "User 2"
      }
```

> Create User - Response Body

```json
      {
        "data": {
          "id": 6,
          "email": "user2@example.com",
          "name": "User 2"
        }
      }
```
### Payload Parameters

Parameter |  Description
--------- |  -----------
name  | User name.
email  | User email.



## Delete an existing user

### HTTP Request

<code> DELETE /users/{id} </code>

> Delete User - Response Body

```json
      {
        "data": {
          "id": 6,
          "email": "user2@example.com",
          "name": "User 2"
        }
      }
```
### Path Parameters

Parameter |  Description
--------- |  -----------
id  | User Id.





# Invitations

# Resources

## Define a category

Resource categories are the objects on which permissions will be assigned. i.e. Folder, repositories,
boards, channels, etc.

From MyLenio just need to follow the steps below:
<ol>
<li>Access your custom app </li>
<li>Add a new category </li>
<li>Check <code>API managed by Lenio</code> field </li>
<li>Speficy the category endpoint field. i.e "/resources", "/boards", "/pages", etc </li>
</ol>

In your API implementation you must implement these groups of endpoints according to your
category endpoint field

Endpoint | Url | Example with "/resources"
------------- | -------------- | ---------------
Create/List | <code>/{category_endpoint}</code> | <code>/resources </code>
Update | <code>/{category_endpoint}/{id}/update</code> | <code>/resources/{id}/update </code>
Delete | <code>/{category_endpoint}/{id}</code> | <code>/resources/{id}/ </code>
Permission Roles | <code>/roles/{category_endpoint}</code> | <code>/roles/resources </code>
List permissions | <code>/{category_endpoint}/{id}/permissions</code> | <code>/resources/{id}/permissions </code>
Edit permissions | <code>/{category_endpoint}/{id}/permissions/{role_id}</code> | <code>/resources/{id}/permissions/{role_id}</code>

<aside class="notice">
The the following specifications will use "/resource" as <code>category_endpoint</code>
</aside>

## Define permission roles

Permission levels or roles need to be provided through an endpoint. This will provide greater flexibility when adding new roles dynamically.

### HTTP Request

<code> GET /roles/resources </code>

> List Roles - Response body

```json
  {
    "data": [
      {"id":1, "name":"Admin", "code":"ADM", "priority":1},
      {"id":2, "name":"Write", "code":"W", "priority":2},
      {"id":3, "name":"Read", "code":"R", "priority":3}
    ]
  }
```

*No parameters are required*

## Add users to a resource

### HTTP Request

<code> PUT /resources/{id}/permissions/{role_id} </code>

>  Add users to a resource - Request Body

```json
      {
        "users": ["10"]
      }
```

>  Add users to a resource - Response Body

```json
  {
    "data": {}
  }
```
### Path Parameters

Parameter |  Description
--------- |  -----------
id  | Resource Id.
role_id  | Permission role id.

### Payload Parameters

Parameter |  Description
--------- |  -----------
users | <code>Array</code> User Ids to add.


## Remove users from a resource

### HTTP Request

<code> DELETE /resources/{id}/permissions/{role_id}?users={users} </code>

>  Remove users from a resource - Response Body

```json
  {
    "data": {}
  }
```
### Path Parameters

Parameter |  Description
--------- |  -----------
id  | Resource Id.
role_id  | Permission role id.

### Query Parameters

Parameter |  Description
--------- |  -----------
users | <code>Array</code> User Ids to add.




## List resources

### HTTP Request
<code> GET /resources </code>

>  List resources - Response Body

```json
  {
    "data":
  [{"id":27,
   "name":"My new resource",
   "description":"Test",
   "is_archived":false,
   "metadata":{},
      "is_archived":false,
      "metadata":{},
      "external_link":"http://smith-gulgowski.co/merle_hartmann",
      "logo_url":nil},
   {
     "id":"2",
    "type":"resource",
    "attributes":
     {"name":"Namfix",
      "description":"Et et occaecati quis.",
      "is_archived":false,
      "metadata":{},
      "external_link":"http://greenholt.io/denna",
      "logo_url":nil}}
      ]
    }

```

*No parameters are required*

## Create a new resource

### HTTP Request

<code> POST /resources </code>

> Create Resource - Request Body

```json
      {
        {"name": "My new resource" , "description": "Test" }
      }
```

> Create Resource - Response Body

```json
   {
    "data":
    {"id":27,
    "name":"My new resource",
    "description":"Test",
    "is_archived":false,
    "metadata":{},
    "external_link":null,
    "logo_url":null,
    "created_at":"2022-05-16T18:08:48.613Z",
    "updated_at":"2022-05-16T18:08:48.613Z"}
   }
```

### Payload Parameters

Parameter |  Description
--------- |  -----------
name | Resource name.
description  | Resource description.

## Update an existing resource

<code> PUT /resources/{id} </code>

> Update Resource - Request Body

```json
      {
        "name": "My new resource" , "description": "Test" ,
        "metadata": {} , "external_link": "", "is_archived": false
      }

```

> Update resource- Response Body

```json
      {
        "data": true
      }
```

### Path Parameters

Parameter |  Description
--------- |  -----------
id  | Resource Id.

### Payload Parameters

Parameter |  Description
--------- |  -----------
name | Resource name.
description  | Resource description.
is_archived  | Boolean value - Set the resource to Archived/unarchived.
metadata  |  <code>Optional</code> Custom JSON will be used by MyLenios.
external_link  | <code>Optional</code> External link setted from MyLenio.

## Delete an existing resource

### HTTP Request

<code> DELETE /resources/{id} </code>

> Delete Resource - Request Body

```json
   {
    "data":
    {"id":27,
    "name":"My new resource",
    "description":"Test",
    "is_archived":false,
    "metadata":{},
    "external_link":null,
    "logo_url":null,
    "created_at":"2022-05-16T18:08:48.613Z",
    "updated_at":"2022-05-16T18:08:48.613Z"}
   }
```

### Path Parameters

Parameter |  Description
--------- |  -----------
id  | Resource Id.

<!--
<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>
<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt; code&gt; </code> blocks to denote code.</aside>

-->
