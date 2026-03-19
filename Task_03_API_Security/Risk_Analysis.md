# Web API Interaction and Automation Analysis
This documentation covers technical testing and automation of REST API requests using curl and terminal-based filtering tools, as shown in your Task 03 analysis.
1. Overview of the Analysis
The task focused on interacting with a RESTful API (jsonplaceholder.typicode.com) to retrieve user data, inspect server headers, and automate repetitive requests using shell scripting. These techniques are foundational for both web development debugging and security auditing (e.g., testing for rate-limiting or API vulnerabilities).
 * Target API: https://jsonplaceholder.typicode.com/users
 * Environment: Termux (Android)
 * Key Tools: curl, grep, and Bash for loops.
2. Technical Explanation of Commands
HTTP Header Inspection (curl -i)
The -i (include) flag tells curl to display the HTTP response headers along with the body.
 * Key Headers Found: * HTTP/2 200: Indicates a successful request over the HTTP/2 protocol.
   * x-ratelimit-limit: Shows the maximum number of requests allowed in a time window (e.g., 1000).
   * content-type: application/json, confirming the data format.
Data Filtering with Grep
By piping curl output to grep, you isolated specific JSON keys from the payload.
 * Command: curl -s [URL] | grep name
 * Function: The -s (silent) flag hides progress bars, and grep filters the raw JSON to show only lines containing the string "name". This is an efficient way to verify specific data points without manually reading large JSON objects.
Request Automation (Bash Loops)
To test server stability or rate-limiting behavior, a shell loop was used to execute multiple requests sequentially.
 * Command: for i in {1..10}; do curl -I -s [URL] | grep "HTTP/"; done
 * Function: This loop sends 10 requests. The -I (head) flag fetches only the headers, making the process faster and using less bandwidth when only the status code (200 OK) is needed for verification.
 * [Excessive Data Exposure Analysis Result](./IMAGE5)
 * [Broken Object Level Authorization(BOLA) Analysis Result](./IMAGE6)
 * [Rate Limiting Analysis Result](./IMAGE7)
3. Analysis of API Behavior
 * Data Integrity: The API correctly returned unique user data for different endpoints (e.g., users/1 for Leanne Graham and users/2 for Ervin Howell).
 * Rate Limiting: The x-ratelimit-remaining header (seen as 999 in Case 1) confirms the server is tracking usage. In a security context, a "Task 03" like this would be used to see if a server properly blocks excessive requests (e.g., a 429 Too Many Requests status).
 * Caching: The cf-cache-status: MISS header indicates the request reached the origin server directly rather than being served from a Content Delivery Network (CDN) cache.
4. Ways of Prevention (API Security)
For Developers/API Providers
 * Implement Rate Limiting: As seen in the headers, strictly limiting requests per IP address prevents Denial of Service (DoS) attacks and brute-force attempts.
 * Input Validation: Ensure that endpoints only accept valid integers for IDs (e.g., users/1) to prevent SQL injection or unauthorized data traversal.
 * CORS Policy: Use Access-Control-Allow-Origin headers to restrict which domains can interact with your API, preventing unauthorized cross-site requests.
For Security Auditors
 * Check for Verbose Headers: Disable headers that leak sensitive server information, such as x-powered-by: Express, which tells an attacker exactly what framework is being used.
 * Monitor for IDOR: Test if changing the ID in the URL allows access to data that should be private (Insecure Direct Object Reference).
 * Enforce HTTPS: Always ensure the API is only accessible over TLS/SSL to prevent man-in-the-middle attacks where data could be intercepted.

