---
layout: page
permalink: /api/apps
---

# Apps
Apps represent your Android applications on Hover’s server. Each app has a unique API token, and a page where you can view information about users’ USSD sessions.

#### App object schema
```json
{
  "data": {
    "id": "783038", // [string] Unique identifier for the object
    "type": "app",  // [string] Object type
    "attributes": { // [hash]   Hash containing object details
      "id": 783038, // [integer] Unique identifier for the object
      "icon": "noimg.svg", // [string] App icon
      "name": "Hover Runner", // [string] App name
      "package_name": "com.usehover.runner", // [string] App's package name
      "token": "e8c1b06780d823c0764476c4b9ce49da", // [string] securely generated app token
      "webhook_url": null, // [string] Webhook URL
      "webhook_secret": null, // [string] securely generated webhook secret
      "organization_id": 62,
      "created_at": "2020-08-05T05:18:49Z",
      "updated_at": "2020-08-05T05:18:49Z",
      "archived_at": null,
      "transactions_count": 0,
      "is_tester": false,
      "apk_url": null
    },
    "relationships": {
      "organization": {
        "data": {
          "id": "62",
          "type": "organization"
        }
      }
    }
  }
}
```

#### Create an app
###### `POST /api/apps`

This endpoint expects a JSON object with the following schema:

```json
{
  "app": {
    "name": "Example App", // [string][required] App name
    "package_name": "com.example.app", // [string][required] Package name
    "webhook_url": "https://example.com/app" // [string][optional] Webhook URL
  }
}
```


Example:

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X POST -d '{ "app": { "name": "Example App", "package_name": "com.example.app", "webhook_url": "https://example.com/app" }}' \
http://localhost:3000/api/apps
```

*Response:*

```json
{
  "data": {
    "id": "43120",
    "type": "app",
    "attributes": {
      "id": 43120,
      "icon": "noimg.svg",
      "name": "Example App",
      "package_name": "com.example.app",
      "token": "49addc974685a64b2c2a75a72b7aa1dc",
      "webhook_url": "https://example.com/app",
      "webhook_secret": "9a62320275d84bb241a0d34d11a1b27a",
      "organization_id": 62,
      "created_at": "2020-08-05T07:12:14Z",
      "updated_at": "2020-08-05T07:12:14Z",
      "archived_at": null,
      "transactions_count": 0,
      "is_tester": false,
      "apk_url": null
    },
    "relationships": {
      "organization": {
        "data": {
          "id": "62",
          "type": "organization"
        }
      }
    }
  }
}
```



#### Retrieve an app
###### `GET api/apps/APP_ID`

Example:
```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
http://localhost:3000/api/apps/43120 
```
The server will respond with an app JSON object.

#### Update an app
###### `PATCH /api/apps/APP_ID`

In this example, we're updating the package name:
```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X PATCH -d '{ "app": { "package_name": "com.example.app.v1" }}' \
https://www.usehover.com/api/apps/43120
```

The server responds with the updated app in JSON format.

#### Archive an app
###### `DELETE api/apps/APP_ID`

This endpoint archives the specified app. You can undo this action by making a PUT request to `api/apps/APP_ID/restore`.

Example:

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X DELETE \
https://www.usehover.com/api/apps/43120
```

The server should respond with the following JSON payload.
```json
{"id":43120,"archived":true}
```

#### Restore an app
###### `PUT api/apps/APP_ID/restore`

This endpoint restores an archived app.

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X PUT \
https://www.usehover.com/api/apps/43120/restore
```

The server will respond with an app object. The `archived_at` attribute should be `null`.