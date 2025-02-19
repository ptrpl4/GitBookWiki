# k6

### links

- docs - [https://k6.io/docs](https://k6.io/docs)
- learn - [https://github.com/grafana/k6-learn](https://github.com/grafana/k6-learn/)

## therms

VU - virtual user (single thread or instance that attempts to simulate a real end user)

gracefulStop - period at the end of the test in which k6 lets iterations in progress finish

```bash
# init new k6 script
k6 new

# run
k6 run script.js
```

example output

```bash
  execution: local
     script: script.js
     output: -

  scenarios: (100.00%) 1 scenario, 10 max VUs, 1m0s max duration (incl. graceful stop):
           * default: 10 looping VUs for 30s (gracefulStop: 30s)


     data_received..................: 2.2 MB 66 kB/s
     data_sent......................: 22 kB  648 B/s
     http_req_blocked...............: avg=54.83ms  min=3µs      med=13µs     max=1.14s    p(90)=22µs     p(95)=503.04ms
     http_req_connecting............: avg=26.57ms  min=0s       med=0s       max=539.59ms p(90)=0s       p(95)=222.97ms
     http_req_duration..............: avg=601.84ms min=253.1ms  med=503.37ms max=4.11s    p(90)=871.48ms p(95)=1.01s   
       { expected_response:true }...: avg=601.84ms min=253.1ms  med=503.37ms max=4.11s    p(90)=871.48ms p(95)=1.01s   
     http_req_failed................: 0.00%  ✓ 0        ✗ 187 
     http_req_receiving.............: avg=26.24ms  min=42µs     med=167µs    max=1.24s    p(90)=438µs    p(95)=7.34ms  
     http_req_sending...............: avg=46.98µs  min=7µs      med=45µs     max=195µs    p(90)=70.4µs   p(95)=81.69µs 
     http_req_tls_handshaking.......: avg=26.18ms  min=0s       med=0s       max=567.51ms p(90)=0s       p(95)=253.11ms
     http_req_waiting...............: avg=575.55ms min=252.88ms med=500.32ms max=3.8s     p(90)=537.99ms p(95)=992.19ms
     http_reqs......................: 187    5.569048/s
     iteration_duration.............: avg=1.65s    min=1.25s    med=1.5s     max=5.11s    p(90)=2.01s    p(95)=2.6s    
     iterations.....................: 187    5.569048/s
     vus............................: 1      min=1      max=10
     vus_max........................: 10     min=10     max=10


running (0m33.6s), 00/10 VUs, 187 complete and 0 interrupted iterations
default ✓ [======================================] 10 VUs  30s
```

## Typical setup

grafana for metrics + k6 for load test

* run grafana docker setup
* add grafana address to k6 docker config
* run k6 docker setup

example structure

* k6
  * scripts
    * api.project.com
      * create.js
      * read.js
      * update.js
      * delete.js
    * api.secure.project.com
