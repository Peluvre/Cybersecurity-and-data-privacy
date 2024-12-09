# Important points to fix as soon as possible

## 1 - Format String Error
Description : This vulnerability occurs when user-supplied input is treated as a command
It was found in the URL: http://localhost:8000/resources using POST method and resource_name in parameter.
Fix : Rewrite background program s o it is excluding bad characheters strings.

## 2 - XSS Vulnerability in JSON Responses
Description : This vulnerability could impact components consuming these JSON responses.
It was found in the URL: http://localhost:8000/resourcesList using GET method and resource_description in parameter.
Fix : Proprely encode all the data included in the JSON responses, avid rendering JSON content in an HTML context.

## 3 - Loosely Scoped Cookies
Description : Cookies are not restricted to a specific domain, which could lead to security issues.
It was found in the URL: http://localhost:8000/login using POST or GET method.
Fix : Limit cookies to a FQND (Fully Qualified Domain Name).

## 4 - Information Disclosure, Suspicious Comments 
Description : The code contain comments which may help an attacker.
It was found in the URL: http://localhost:8000/revervation using GET method.
Fix : Remove all the comments containing sensitive informations.

## 5 - Session Management Identified
Description : Session management tokens were identified in the response headers.
It was found in the URL: http://localhost:8000/login using POST or GET method and session_id parameter.
Fix : Ensure session tokens are properly secured.
