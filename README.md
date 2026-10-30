# K6 lab

Performance testing exercises using k6 and Grafana's QuickPizza demo. Install k6 (for example, `brew install k6` on macOS), then run a script with `k6 run <script>`. Measured results are in `RESULTS.md`; HTML reports are in `reports/`.

To regenerate an HTML report, for example:

```sh
K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_EXPORT=reports/a2-average.html k6 run a2-average.js
```

The staged tests use zero graceful stop and ramp-down time so the load stays within the lab's duration limits. An iteration still running at a ramp-down may be interrupted.
