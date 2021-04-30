## CORS 
Same origin policy works only for scripts
Different origins are
1. Domain
2. Protocol
3. Port

### Simple requests
For simple requests CORS works without OPTION request
1. One of methods(GET,POST,HEAD)
2. Allowed headers for Content Type
    * application/x-www-form-urlencoded
    * multipart/form-data
    * text/plain

Simple request just sends Origin and expects **Access-Control-Allow-Origin** to be the same as Origin

### Preflight

1. Clients sends OPTION request with following headers
    * **Access-Control-Request-Method: POST** - method
	* **Access-Control-Request-Headers: X-PINGOTHER, Content-Type** - if any non standart headers
2. Server responds
   * Access-Control-Allow-Origin: https://foo.example
   * Access-Control-Allow-Methods: POST, GET, OPTIONS
   * Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
   * Access-Control-Max-Age: 86400 - how long browser could cache the response and don't send OPTIONS again

By default client doesn't send cookies

1. Client sends request with **Cookie: pageAccess=2**
2. Server should respond with this header **Access-Control-Allow-Credentials: true**
