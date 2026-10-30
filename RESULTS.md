# K6 lab results

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
| A5 | Soak | 2 | | | | | |
