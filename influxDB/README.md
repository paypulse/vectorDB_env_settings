# INFLUX DB
 
 ```
    [실행]
     docker-compose -f .\docker-compose-influx.yml up -d 

    [UI 확인]
     http://localhost:8086 

    [내부 토큰 값]
     docker exec -it infulxdb influx auth list 
 ```