---
title: API
layout: rancher-api-v1-default-v1.2
version: v1.2
lang: en
apiVersion: v1
---

## serviceProxy



### Resource Fields

#### Writeable Fields

Field | Type | Create | Update | Default | Notes
---|---|---|---|---|---
port | int | Optional | - | 80 | 
scheme | enum | Optional | - | http | The options are `http`, `https`.
service | string | Yes | - | - | 


#### Read Only Fields

Field | Type   | Notes
---|---|---
token | string  | 
url | string  | 


<br>
