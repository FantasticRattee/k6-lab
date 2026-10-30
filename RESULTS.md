# K6 lab results

## Part 0

- Step 0.1, http_req_duration: 269.35 ms.
- Step 0.2: 75 requests, 3.704 requests/s, p(95) 320.57 ms. Each user waits one second and also spends time on the request, so each completes fewer than one request per second.
- Step 0.6: the default p(95) < 1,500 ms run exited 0 (p(95) 404.63 ms). The stricter p(95) < 300 ms run exited 99 (p(95) 427.53 ms).
