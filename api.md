---
layout: page
permalink: /api
---

# Configuration API Reference

The Hover API allows you to create, edit, and download actions and parsers for your account. is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). All API access is over HTTPS, and accessed from [https://www.usehover.com/api](https://www.usehover.com/api). All data is sent and received as JSON.

Blank fields return as `null` and timestamps return in ISO 8601 format: `2020-02-10T16:13:55Z`.

#### Authentication
The Hover API uses [JWT](https://jwt.io/) to authenticate requests. Use the email and password for your Hover dashboard login.

##### Authentication Flow

```bash
curl \
-H "Content-Type: application/json" \
-X POST -d '{ "email": "your@email.adr", "password": "yourPassword"}' \
https://www.usehover.com/api/authenticate
```

This returns a JSON response:
```json
{ "auth_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c" }
```

This is a JWT token valid for 2 hours. As long as it's still valid this token can be used to make requests on the Hover API by passing the header like this: `Authorization: JWT-TOKEN`.

```bash
 curl \
 -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c" \
-H "Content-Type: application/json" \
https://www.usehover.com/api/actions?organization_id=1
```
