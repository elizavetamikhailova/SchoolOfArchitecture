
liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_06$ wrk -d 10 -t 10 -c 10 --latency -s ./get.lua http://localhost:8081/
Running 10s test @ http://localhost:8081/
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    42.94ms    9.30ms 108.65ms   79.91%
    Req/Sec    23.26      5.19    40.00     63.40%
  Latency Distribution
     50%   42.18ms
     75%   47.11ms
     90%   52.91ms
     99%   71.95ms
  2329 requests in 10.02s, 676.85KB read
Requests/sec:    232.48
Transfer/sec:     67.56KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_06$ wrk -d 10 -t 10 -c 10 --latency -s ./get.lua http://localhost:8082/
Running 10s test @ http://localhost:8082/
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    14.97ms    4.46ms  68.12ms   88.90%
    Req/Sec    67.42     10.89   100.00     67.30%
  Latency Distribution
     50%   14.19ms
     75%   15.92ms
     90%   18.17ms
     99%   33.30ms
  6748 requests in 10.04s, 1.91MB read
Requests/sec:    672.38

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_06$ wrk -d 10 -t 1 -c 1 --latency -s ./get.lua http://localhost:8081/
Running 10s test @ http://localhost:8081/
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     4.46ms    2.48ms  50.59ms   96.55%
    Req/Sec   233.09     29.56   290.00     72.00%
  Latency Distribution
     50%    4.07ms
     75%    4.61ms
     90%    5.33ms
     99%   14.16ms
  2325 requests in 10.02s, 653.91KB read
Requests/sec:    232.06
Transfer/sec:     65.27KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_06$ wrk -d 10 -t 1 -c 1 --latency -s ./get.lua http://localhost:8082/
Running 10s test @ http://localhost:8082/
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     9.04ms   37.49ms 346.33ms   95.97%
    Req/Sec   512.91     95.76   690.00     85.42%
  Latency Distribution
     50%    1.80ms
     75%    2.14ms
     90%    2.83ms
     99%  238.95ms
  4944 requests in 10.02s, 1.41MB read
Requests/sec:    493.55
Transfer/sec:    144.59KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_06$ wrk -d 10 -t 50 -c 50 --latency -s ./get.lua http://localhost:8081/
Running 10s test @ http://localhost:8081/
  50 threads and 50 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   200.20ms   55.85ms 218.06ms   91.67%
    Req/Sec     3.58      2.02    10.00     91.67%
  Latency Distribution
     50%  216.57ms
     75%  217.13ms
     90%  217.26ms
     99%  218.06ms
  12 requests in 10.10s, 3.54KB read
Requests/sec:      1.19
Transfer/sec:     359.22B

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_06$ wrk -d 10 -t 50 -c 50 --latency -s ./get.lua http://localhost:8082/
Running 10s test @ http://localhost:8082/
  50 threads and 50 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   137.49ms   99.11ms 232.00ms  100.00%
    Req/Sec     7.50      2.64    10.00    100.00%
  Latency Distribution
     50%  230.39ms
     75%  231.77ms
     90%  232.00ms
     99%  232.00ms
  10 requests in 10.10s, 2.92KB read
Requests/sec:      0.99
Transfer/sec:     296.47B