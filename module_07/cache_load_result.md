MariaDB

|Type             |Latency           |RPS          |
|:----------------|:-----------------|:------------|
|Without cache, 1 thread, 1 connection|4.46ms |233.09|
|With cache, 1 thread, 1 connection|9.04ms |512.91|
|Without cache, 10 thread, 10 connection|42.94ms |23.26|
|With cache, 10 thread, 10 connection|14.97ms |67.42|
|Without cache, 50 thread, 50 connection|200.20ms |3.58|
|With cache, 50 thread, 50 connection|137.49ms |7.50|

MongoDB

|Type             |Latency           |RPS          |
|:----------------|:-----------------|:------------|
|Without cache, 1 thread, 1 connection|2.22ms |509.19|
|With cache, 1 thread, 1 connection|0.99ms |1.06k|
|Without cache, 10 thread, 10 connection|12.46ms |82.76 |
|With cache, 10 thread, 10 connection|7.22ms  |143.09|
|Without cache, 50 thread, 50 connection|44.92ms |22.25|
|With cache, 50 thread, 50 connection|30.91ms |32.92|

# Вывод: 
    В данных условиях MongoDB показала себя лучше, она эффективно обрабатывает операции чтения большого объема данных и предоставляет высокую производительность при параллельной работе с несколькими запросами. В то время как MariaDB обеспечивает хорошую производительность при работе с небольшими объемами структурированных данных и хорошо подходит для использования в приложениях, где требуется высокая консистентность данных и поддержка транзакций. По результатам данного нагрузочного тестирования можно судить только о работе данного конкретного сервиса, но стоит учитывать, что сервисы это не только чтение 100к записей, но еще и их измение и добавление и для того, чтобы оценить какая база данных куда подойдет лучше нужно анализировать систему в целом, а так же требования к ней

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_07$ wrk -d 10 -t 10 -c 10 --latency -s ./get.lua http://localhost:8081/
Running 10s test @ http://localhost:8081/
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    12.46ms    6.04ms  79.38ms   86.58%
    Req/Sec    82.76     21.18   151.00     68.50%
  Latency Distribution
     50%   10.96ms
     75%   13.90ms
     90%   18.77ms
     99%   35.58ms
  8269 requests in 10.02s, 2.25MB read
Requests/sec:    825.32
Transfer/sec:    230.10KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_07$ wrk -d 10 -t 10 -c 10 --latency -s ./get.lua http://localhost:8082/
Running 10s test @ http://localhost:8082/
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     7.22ms    3.71ms  70.61ms   87.70%
    Req/Sec   143.09     25.33   230.00     71.30%
  Latency Distribution
     50%    6.53ms
     75%    8.15ms
     90%   10.41ms
     99%   18.97ms
  14282 requests in 10.03s, 3.84MB read
Requests/sec:   1423.89
Transfer/sec:    391.59KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_07$ wrk -d 10 -t 1 -c 1 --latency -s ./get.lua http://localhost:8081/
Running 10s test @ http://localhost:8081/
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     2.22ms    1.79ms  23.20ms   92.20%
    Req/Sec   509.19    101.61   680.00     69.00%
  Latency Distribution
     50%    1.70ms
     75%    2.09ms
     90%    3.29ms
     99%   10.77ms
  5077 requests in 10.01s, 1.36MB read
Requests/sec:    507.06
Transfer/sec:    138.65KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_07$ wrk -d 10 -t 1 -c 1 --latency -s ./get.lua http://localhost:8082/
Running 10s test @ http://localhost:8082/
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     0.99ms  690.53us  17.20ms   96.86%
    Req/Sec     1.06k   119.70     1.28k    73.00%
  Latency Distribution
     50%    0.87ms
     75%    1.01ms
     90%    1.21ms
     99%    3.91ms
  10570 requests in 10.00s, 2.84MB read
Requests/sec:   1056.61
Transfer/sec:    290.98KB


liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_07$ wrk -d 10 -t 50 -c 50 --latency -s ./get.lua http://localhost:8081/
Running 10s test @ http://localhost:8081/
  50 threads and 50 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    44.92ms    8.23ms 126.22ms   74.61%
    Req/Sec    22.25      4.88    40.00     72.23%
  Latency Distribution
     50%   44.93ms
     75%   49.63ms
     90%   54.10ms
     99%   67.52ms
  11122 requests in 10.03s, 3.01MB read
Requests/sec:   1109.24
Transfer/sec:    307.62KB

liza@DESKTOP-PKFJ2IG:~/SchoolOfArchitecture/module_07$ wrk -d 10 -t 50 -c 50 --latency -s ./get.lua http://localhost:8082/
Running 10s test @ http://localhost:8082/
  50 threads and 50 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    30.91ms    9.33ms 145.25ms   88.44%
    Req/Sec    32.92      6.44    60.00     58.52%
  Latency Distribution
     50%   29.85ms
     75%   33.69ms
     90%   38.26ms
     99%   53.17ms
  16436 requests in 10.10s, 4.46MB read
Requests/sec:   1627.33
Transfer/sec:    452.23KB

