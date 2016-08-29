# API

The API is designed to be fully RESTful. We make use of:

```
POST
GET 
PUT
DELETE
```

## Requests and responses

All requests return JSON. For a successfull response, the following is returned:

```
{
  "response": "Response data, often as a JSON array"
}
```

For a response that resulted in an error, the following is returned:

```
{
  "error": "A detailed error message."
}
```

## Testing the API

We use [Postman](https://getpostman.com) to do the API requests. You can import the latest 
Postman [collection here](https://bvnk.co/Bvnk.postman_collection.json).

If developing locally, please either change the url from `bvnk.co:8443` to your local instance. Alternatively, 
add the following in your `/etc/hosts` file:

```
[IP address]    bvnk.co

# Example:
# 127.0.0.1     bvnk.co
```
