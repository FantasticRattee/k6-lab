# K6 lab results

Runs were made on 30 October 2026 against the shared QuickPizza demo using k6 v2.2.0.

## Part 0

- Step 0.1, http_req_duration: 269.35 ms.
- Step 0.2: 75 requests, 3.704 requests/s, p(95) 320.57 ms. Each user waits one second and also spends time on the request, so each completes fewer than one request per second.
- Step 0.6: the default p(95) < 1,500 ms run exited 0 (p(95) 404.63 ms). The stricter p(95) < 300 ms run exited 99 (p(95) 427.53 ms).
- Step 0.7: all three rules passed: p(95) 400.88 ms, p(99) 706.64 ms, and failed requests 0%. Changing the p(95) limit from 1,000 ms to 1 ms made the test fail with exit code 99.
- Step 0.8: the exclusions used lowercase names, but the API's catalog lists `Pepperoni` and `Knife`. Before the fix, 9 of 75 pizzas contained pepperoni and 23 of 75 required a knife. After using the catalog's exact names, all 225 checks passed across 75 requests. Repeated random recommendations exposed failures that a single request could miss by chance.

## Part A results

| Step | Test type | Users | Req/s | p(95) (ms) | Max (ms) | Failed | Checks |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| A1 | Smoke | 1 | 0.534 | 377.44 | 404.78 | 0% | 100% |
| A2 | Average load | 3 | 1.261 | 385.61 | 432.50 | 0% | 100% |
| A3 | Stress | 5 | 2.144 | 397.31 | 1378.04 | 0% | 100% |
| A4 | Spike | 5 | 1.014 | 411.58 | 506.89 | 0% | 100% |
| A5 | Soak | 2 | 0.942 | 412.02 | 1376.88 | 0% | 100% |

## Part B

1. See the Part A table above.
2. Each virtual user waits one second and also spends time on the request, so five users make fewer than five requests per second.
3. Overall p(95) increased from 385.61 ms at 3 users to 397.31 ms at 5 users. Both runs met their thresholds. These small tests do not establish the site's maximum operational envelope.
4. p(95) increased from about 352 ms before the spike (0–30 s) to 442 ms during the five-user hold (35–65 s). A 10-second window around 10–20 seconds after the load returned to one user was near the earlier level, at about 324 ms. It had only a few requests and later windows rose again, so a stable recovery time cannot be established from this run.
5. Comparing the first and last two minutes of the steady two-user hold, p(95) increased from 386.22 ms to 438.28 ms, while HTTP failures stayed at 0% in both windows. This single short run does not establish a sustained degradation. A longer soak could reveal a memory leak.
6. p(95) shows the response time experienced by 95% of requests; an average can hide slower requests.
7. Internet distance and network latency differ from a local lab, and the shared site can have other users or changing background load.
8. Hundreds of users can overload a shared service and disrupt others; testing without the owner's permission is not acceptable.
9. Vague: “ChatGPT should respond quickly.” Testable example: assume a non-streaming test interface that returns a JSON `answer` field. For the prompt “What is 2 + 2?”, with 3 concurrent users for 30 seconds, p(95) of HTTP request duration must be below 3,000 ms, fewer than 1% of HTTP requests may fail, and more than 99% of checks must pass. Each request has one combined check: status 200 and `answer` is `"4"`. This is a proposed requirement with a hypothetical interface, not a test of ChatGPT.

   ```js
   thresholds: {
     http_req_duration: ['p(95)<3000'],
     http_req_failed: ['rate<0.01'],
     checks: ['rate>0.99'],
   }
   ```
