# DNS prefetch
Syntax

```
&lt;link rel=&quot;dns-prefetch&quot; href=&quot;https://fonts.gstatic.com/&quot; &gt;

```
When not to use
1. For fonts,css,scripts of another domain - if js script doesn't call other domains then it's not necessary

When to use
1. Script uses other domains inside
2. The domain will be required within a minute of document being loaded

# Preconnect
**dns-fetch** just uses DNS, but if resource is really important then it's recommended to use **preconnect**
Syntax
```
&lt;link rel=&quot;preconnect&quot;
     href=&quot;https://example.net/&quot;&gt;

```
In this case browser will
1. Resolve DNS
2. Set up TCP connection
3. TLS handshake(if https)

When to use
1. The domain is chained dependency (rest call inside script tag)
2. The domain is expected to be required within 10 seconds of document loading
