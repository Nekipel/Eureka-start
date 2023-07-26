Цель: Практика работы с серверами, связь клиента и сервера

Задание:  для клиента добавь следующие настройки на стороне сервера:

не хотим получать информацию реестра от Eureka Server; 

время ожидания (одна минута) до истечения тайм-аута соединения с Eureka Server;

каждые 15 секунд запрашивать изменения об информации с сервера;

запрети dns и укажи явный маршрут к серверам eureka (serviceUrl);

если возникнут проблемы в сети или наш сервер выйдет из строя, мы хотим запретить клиент;

использовать сервер eureka в той же зоне;

для клиента включим healthcheck;

установить время, по которому сервер будет ожидать эхо запрос от клиента о том, что он жив (20 секунд);

если Eureka Server не видел обновления в течение 70 секунд, он удаляет экземпляр из своего реестра.

На стороне клиента:

укажи явный маршрут к серверу eureka (serviceUrl);

используй IP адрес;

укажи интервал в 35 секунд, через который клиент шлет эхо запрос на сервер.

Запусти вначале сервер, а затем — клиент, убедитесь, что он был успешно зарегистрирован в Eureka Server, это можно будет увидеть на UI дашборде Eureka.
Поэкспериментируй:, запусти вначале клиент а затем — сервер и посмотри,что будет в логах.




СЕРВЕР
spring:
  application:
    name: eureka-server

server:
  port: ${PORT:8761}

eureka:
  client:
    fetch-registry: false # 1 не хотим получать информацию реестра от Eureka Server
    eureka-server-connect-timeout-seconds: 60 # 2 время ожидания (1 минута) до истечения тайм-аута соединения с Eureka Server
    eureka-service-url-poll-interval-seconds: 15 # 3 каждые 15 секунд запрашивать изменения об информации с сервера
    use-dns-for-fetching-service-urls: false # 4 запретите dns и укажите явный маршрут к серверам eureka (serviceUrl)
    prefer-same-zone-eureka: true # 6.1 использовать сервер eureka в той же зоне
    healthcheck: # 7 для клиента включим healthcheck
      enabled: true
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
    register-with-eureka: false


  server:
    renewal-percent-threshold: 0.85 # 5 если возникнут проблемы в сети или наш сервер выйдет из строя мы хотим запретить клиент

  instance:
    hostname: my-eureka-server.com
    metadata-map:
      zone: zone_A # 6.2 использовать сервер eureka в той же зоне
    lease-renewal-interval-in-seconds: 20 # 8 установить время, по которому сервер будет ожидать эхо запрос от клиента о том что он жив (20 секунд)
    lease-expiration-duration-in-seconds: 70 # 9 Если Eureka Server не видел обновления в течение 70 секунд, он удаляет экземпляр из своего реестра

КЛИЕНТ
eureka:
  client:
    serviceUrl:
      defaultZone: ${EUREKA_URI:http://localhost:8761/eureka} # 1 укажите явный маршрут к серверу eureka (serviceUrl)
    eureka-service-url-poll-interval-seconds: 35 # 3 укажите интервал в 35 сек через который клиент шлет эхо запрос на сервер
  instance:
    preferIpAddress: true # 2 использовать IP адрес


https://cloud.spring.io/spring-cloud-static/Dalston.SR5/multi/multi__appendix_compendium_of_configuration_properties.html
