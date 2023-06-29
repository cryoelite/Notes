- There are 2 requirements for CORS loaded resources to be allowed loading in our own [[HTML]] document by the [[Brower]]
  
  The resource/ [[HTML Element]] must have the ``crossorigin="<value>"`` attribute
  If the value to this attribute is "anonymous" and the server responds to our [[HTTP]] request with a response with header ``Access-Control-Allow-Origin`` with value "*" then it is allowed.
  
  If the value to this attribute is "use-credentials" and the server responds with ``Access-Control-Allow-Origin`` with value "*" or "<our domain name>" as well as ``Access-Control-Allow-Credentials`` with value "true" then it is allowed.