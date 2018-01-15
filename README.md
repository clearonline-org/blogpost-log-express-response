# How to log an Expressjs response body

Both Express Request and Response are NodeJs Streams. With this in mind
An Express Request is readable NodeJS Stream, which means that it provide a *data* event that a developer can listen for in order to see the data/payload being received from the client.

The Express Response on the other hand, is implemented as a writable NodeJS Stream. And for some reasons, writable streams do not provide a a *data* event. Therefore, there is no straightforward way of capturing the data being written (ex: if you want to log them).

After checking how Express js implements the `send` method, i came up with a naive way of capturing the response payload.

## Steps

* Create a middleware 
```javascript
app.use((req, res, next) => {
  // ... response payload capture logic
})
// your api handler here
```

* override the `res.end` function
```javascript
app.use((req, res, next) => {
    res.end = function(chunk) {
        console.log('[res.end] chunk', typeof chunk, chunk)
        end.apply(res, arguments)
      }
    next()
})
```

## NOTES

* This piece of code was tested with `express@4.16.2`
* This piece of code works if you use `res.send` or `res.json`
* This piece of code does NOT works if you use `res.sendFile` (in this case you will need to [do more work](https://blog.clearonline.org) )
