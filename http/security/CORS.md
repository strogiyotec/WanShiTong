## CORS 
Different origins are

1. Domain
2. Protocol
3. Port

### Simple requests
Simple requests don't triiger cors preflight

1. One of methods(GET,POST,HEAD)
2. Allowed headers
    * application/x-www-form-urlencoded
    * multipart/form-data
    * text/plain

Simple request just sends Origin and expect **Access-Control-Allow-Origin** to be the same as Origin

### Preflight

1. Clients sends OPTION request
    * **Access-Control-Request-Method: POST** - method
	* **Access-Control-Request-Headers: X-PINGOTHER, Content-Type**
2. Server responds
   * Access-Control-Allow-Origin: https://foo.example
   * Access-Control-Allow-Methods: POST, GET, OPTIONS
   * Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
   * Access-Control-Max-Age: 86400

By default client doesn't send cookies

1. Client sends request with **Cookie: pageAccess=2**
2. Server should respond with this header **Access-Control-Allow-Credentials: true**
