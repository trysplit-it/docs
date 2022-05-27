---
layout: default
title: API
nav_order: 3
---
# API Reference

Our API can be accessed from our domain at these endpoints.

## Events

Our Events are JSON Objects that contain basic information about any upcoming trips. The fields of this object are listed below, along with their types and hyperparameters:

| Parameter     | Type   | Description                                    | 
| ------------- | ------ | ---------------------------------------------- |         
| title         | String | The title of an event. *Default: ""*           |
| startDate     | Date   | The date of the event.                         |
| passengerList | Array  | Array of User Objects.                         |
| departure     | Object | Contents specified below. **required.**        |
| destination   | Object | Contents specified below. **required.**        |
| owner         | User   | The owner of the event. **required.**          |
| notes         | String | Any notes by the owner. **required.**          |
| capacity      | Number | The total capacity of the event. **required.** |

**Departure Object:**

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| name      | String | Name of the departure location. **required.**    |
| address   | String | Address of the departure location.               |
| location  | Array  | Array of numbers (2) to specify the coordinates. |

**Destination Object:**

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| name      | String | Name of the destination location. **required.**  |
| address   | String | Address of the destination location.             |
| location  | Array  | Array of numbers (2) to specify the coordinates. |

### Event Routing

Here is a summary of all of our routes for events. Note that all of these events are exclusive to requests with a bearer token.

| Request | Route              | Auth         | Description                                     |
| ------- | ------------------ | ------------ | ----------------------------------------------- |
| GET     | /api/events        | Bearer Token | Gets a list of events with specified filter.    |
| GET     | /api/events/:id    | Bearer Token | Gets the event specified.                       |
| GET     | /api/events/joined | Bearer Token | Gets a list of events joined by the token user. |
| POST    | /api/events        | Bearer Token | Creates a new event with the params.            |
| DELETE  | /api/events/:id    | Bearer Token | Deletes the event specified.                    |
| PUT     | /api/events/:id    | Bearer Token | Updates the event specified.                    |

**Requests & Responses:**

```
GET /api/events

(All optional parameters for search)
req =  { title, start, end, departure, destination, notes }
```

| Code | Response               |
| ---- | ---------------------- |
| 200  | Array of Event Objects |
| 403  | Invalid/missing token  |

```
GET /api/events/joined

```

| Code | Response                                    |
| ---- | ------------------------------------------- |
| 200  | Array of Event Objects joined by token user |
| 403  | Invalid/missing token                       |

```
GET /api/events/:id

req.params = { id }
```

| Code | Response                     |
| ---- | ---------------------------- |
| 200  | Single Event Object          |
| 403  | Invalid/missing token        |
| 404  | Error: event does not exist  |

```
POST /api/events

req.body = { title, startDate, departure, owner, notes, capacity }
```

| Code | Response                                  |
| ---- | ----------------------------------------- |
| 201  | Single Event Object posted successfuly    |
| 400  | Error: No departure specified             |
| 400  | Error: No destination specified           |
| 400  | Error: Capacity less than 2               |
| 403  | Invalid/missing token                     |

```
DELETE /api/events/:id

req.params = { id }
```

| Code | Response                                |
| ---- | --------------------------------------- |
| 200  | Single Event Object deleted sucessfully |
| 403  | Error: Event still has passengers       |
| 403  | Invalid/missing token                   |
| 404  | Error: event does not exist             |

```
PUT /api/events/:id

req.params = { id }
```

| Code | Response                                 |
| ---- | ---------------------------------------- |
| 200  | Single Event Object updated successfully |
| 400  | Error: user already registered           |
| 400  | Error: add and remove simultaneous call  |
| 400  | Error: user not registered               |
| 400  | Error: User updating event not provided  |
| 403  | Error: Only owner can update event       |
| 403  | Invalid/missing token                    |
| 404  | Error: event does not exist              |


## Users

Our Users are JSON Objects that contain information extracted from SSO and the events that the users own/joined. None of these routes (except PUT) can actually be accessed by users, as they are only for administrative purposes. The fields of this object are listed below, along with their types and hyperparameters:

| Parameter     | Type    | Description                                      | 
| ------------- | ------- | ------------------------------------------------ |         
| jhed          | String  | JHED of a user. **required.** **unique.**        |
| email         | String  | Email of a user. **required.** **unique.**       |
| username      | String  | Username of a user. **required.** **unique.**    |
| isAdmin       | Boolean | Self-explanatory. **required.** *defalult:false* |
| eventsOwned   | Array   | Array of Event Objects. **required.**            |
| eventsJoined  | Array   | Array of Event Objects. **required.**            |

### User Routing

| Request | Route          | Auth         | Description                         |
| ------- | -------------- | ------------ | ----------------------------------- |
| GET     | /api/users     | Admin        | Gets a list of all users.           |
| GET     | /api/users     | Bearer Token | Gets the user specified.            |
| POST    | /api/users     | Admin        | Creates a new user with the params. |
| DELETE  | /api/users/:id | Admin        | Deletes the user specified.         |
| PUT     | /api/users/:id | Bearer Token | Updates the user specified.         |

```
GET /api/users/all

```

| Code | Response               |
| ---- | ---------------------- |
| 200  | Array of User objects  |
| 403  | Invalid/missing token  |
| 500  | Invalid authorization  |

```
GET /api/users

```

| Code | Response               |
| ---- | ---------------------- |
| 200  | Single User object     |
| 200  | TODO                   |
| 403  | Invalid/missing token  |

```
POST /api/users

req.body = { username, jhed, email }
```

| Code | Response               |
| ---- | ---------------------- |
| 201  | Single User object     |
| 403  | Invalid/missing token  |

```
DELETE /api/users/:id

```

| Code | Response                            |
| ---- | ----------------------------------- |
| 200  | Single User object                  |
| 403  | Invalid/missing/unauthorized token  |
| 500  | Invalid authorization               |

```
PUT /api/users/

```

| Code | Response               |
| ---- | ---------------------- |
| 200  | Single User object     |
| 403  | Invalid/missing token  |

## SSO

Our SSO utilizes 4 endpoints, three for authentication with an external server (JHU) and one for token verification.

### SSO Routing

Here is a summary of all our routes for SSO and tokenization.

| Request | Route               | Auth         | Description                                  |
| ------- | ------------------- | ------------ | -------------------------------------------- |
| GET     | /jhu/metadata       | None         | Gives the XML file for SAML2 trust exchange. |
| GET     | /jhu/login          | None         | Redirects the user to JHU's SSO service.     |
| POST    | /jhu/login/callback | None         | Callback from JHU's SSO service.             |
| POST    | /verify             | None         | Verifies the token given from body params.   |

```
GET /jhu/metadata

```

| Code | Response                                |
| ---- | --------------------------------------- |
| 200  | XML file returned successfully.         |

```
GET /jhu/login

```

RESPONSE CODES DETERMINED BY JHU IT.

```
GET /jhu/login/callback

```

RESPONSE CODES DETERMINED BY JHU IT.

```
GET /verify

req.body = { token }
```

| Code | Response                  |
| ---- | ------------------------- |
| 200  | Verification successful.  |
| 403  | Invalid or expired token. |