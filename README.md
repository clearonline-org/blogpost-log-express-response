# How to log an Expressjs response body

Both Express Request and Response are NodeJs Streams. With this in mind
An Express Request is readable NodeJS Stream, which means that it provide a *data* event that a developer can listen for in order to see the data/payload being received from the client.

The Express Response on the other hand, is implemented as a writable NodeJS Stream. And for some reasons, writable streams do not provide a a *data* event. Therefore, there is no straightforward way of capturing the data being written (ex: if you want to log them).

After checking how Express js implements the `send` method, i came up with a naive way of capturing the response payload.
