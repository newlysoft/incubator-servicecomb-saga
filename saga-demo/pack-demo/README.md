# Pack Transaction Demo
This demo simulates a booking application including three services:
* pack-booking
* pack-car
* pack-hotel

## Prerequisites
You will need:
1. [JDK 1.8][jdk]
2. [Maven 3.x][maven]
3. [Docker][docker]
4. [Docker compose][docker_compose]

[jdk]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[maven]: https://maven.apache.org/install.html
[docker]: https://www.docker.com/get-docker
[docker_compose]: https://docs.docker.com/compose/install/

## Running Demo
1. run the following command to create docker images in saga project root folder.
```
mvn package -DskipTests -Pdocker -Pdemo
```

2. start application up in saga/saga-demo/pack-demo with the following command
```
docker-compose up
```

## User Requests
1. Booking 2 rooms and 2 cars, this booking will be OK.
```
curl -X POST http://{docker.host.ip}:8083/booking/test/2/2
```
Check the hotel booking status with
```
curl http://{docker.host.ip}:8081/bookings
```
Check the car booking status with
```
curl http://{docker.host.ip}:8082/bookings

```

2. Booking 3 rooms and 2 cars, this booking will case the hotel order failed and trigger the compansate operation with car booking.
```
curl -X POST http://{docker.host.ip}:8083/booking/test/3/2
```
Check the hotel booking status with
```
curl http://{docker.host.ip}:8081/bookings
```
Check the car booking status with
```
curl http://{docker.host.ip}:8082/bookings
```
The second car booking will be marked with **cancel:true**


**Note** transactions and compensations implemented by services must be idempotent.
