# K6 lab results

## Part 0
- Step 0.1, http_req_duration: avg=278.43ms min=278.43ms med=278.43ms max=278.43ms p(90)=278.43ms p(95)=278.43ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 75, 3.709635/s, p(95)=418.19ms, because the sleep(1) line in the code. Because of that wait, each user sends less than 1 request per second. So 5 users send less than 5 requests per second.
- Step 0.6, exit code of the normal run and of the run with P95=300: 0 and 99
- Step 0.7, rules that passed, p(95), p(99), exit code after your change: all 3, p(95)=413.26ms, p(99)=418.93ms, 99
- Step 0.8, what was wrong, and how the checks showed it: pepperoni and knife needs to be capitalize, the pizza that get picked can be one with no pepperoni and knife already, so many request show the instance when the wrong pizza show up.

## Part A results
| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     |   0.482961    |  376.66ms   |    388.3ms    |    0.00%    |    100.00%    |
| A2   | Average load | 3     |   1.298998    | 418.47 ms | 583.74 ms | 0.00% | 100.00% |
| A3   | Stress       | 5     |   2.152134    | 404.13 ms | 560.08 ms | 0.00% | 100.00% |
| A4   | Spike        | 5     |    1.004996   | 432.57 ms | 630.68 ms | 0.00% | 100.00% |
| A5   | Soak         | 2     |   0.97104    | 496.91 ms | 1336 ms | 0.00% | 100.00% |  

## Part B
1. Above
2. Because the sleep(1) line in the code. Because of that wait, each user sends less than 1 request per second. So 5 users send less than 5 requests per second.
3. No. The p(95) was 418.47 ms at 3 users and 404.13 ms at 5 users, so it did not grow, although the request rate went up. Both loads are well inside the operational envelope.
4. The response time was back to normal within 20 to 30 seconds.
5. No. A soak test of several hours could reveal a memory leak.
6. The average hides the slowest users. The p(95) shows how slow the slowest requests are.
7. 1 a public site is reached over the internet, so the network delay changes. 2, QuickPizza is shared with everyone learning K6, and their traffic can affects the numbers.
8. It can slow down or crash the site for real users, it looks like a denial-of-service attack, and it can be illegal.
9. Vague: "The food search in my delivery app should be fast."
   Testable: with 3 users for 30 seconds, 95% of search requests answer within
   1 second, 99% within 1.5 seconds, and fewer than 1% fail.
     thresholds: {
       http_req_failed: ['rate<0.01'],
       http_req_duration: ['p(95)<1000', 'p(99)<1500'],
     },