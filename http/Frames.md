## Problem with Http1
With http1 client has to read whole data from socket. It also hard to predict the amount of data.

## Http2 frames
Frames have a length in each packet. Also frames have a stream identifier which prevent **Head of line blocking**

## Headers compressions
**Example**<br>
Client sends three headers
```
header1:
header2:
header3:
```
Client creates an index for each header
`1-header1,2-header2`. Then server saves these indexes<br>
Finally , client sends indexes instead of headers and server will find corelated headers.

### Resources
1. [Specification](https://www.rfc-editor.org/rfc/rfc7541.txt)
