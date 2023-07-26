Режим самосохранения 

Eureka Server перейдет в режим самосохранения, если обнаружит, что зарегистрированное количество клиентов превысило ожидаемое количество и прервало соединения для связи с сервером.
Это было сделано для того, чтобы гарантировать, что какие-либо сетевые события (большие нагрузки или проблемы в сети) не уничтожат данные реестра Eureka Server.
Чтобы установить порог самосохранения, надо указать свойство в файле properties.
eureka:
 renewalPercentThreshold: 
Чтобы отключить режим самосохранения, необходимо:
eureka:
 enableSelfPreservation: false
Как уже упоминалось выше, клиенты по умолчанию пытаются установить связь с сервером в той же зоне. Если возникают проблемы при общении с сервером или если сервер не существует в той же зоне, клиенты переключаются на серверы в других зонах. Это важно понимать!


Укажите, следует ли  экземпляру пытаться использовать сервер eureka в той же зоне из-за задержки по сети или по другой причине, или нет.

eureka:
 client:
   prefer-same-zone-eureka: false

Режим самосохранения (Self-Preservation Mode) в Eureka Server предназначен для обеспечения надежности работы системы регистрации и обнаружения микросервисов в случае временных сбоев в сети или на серверах.

Когда включен режим самосохранения, Eureka Server автоматически переключается на локальный режим работы, если не получает информацию от других серверов в кластере в течение определенного времени. В этом режиме сервер продолжает работу, используя только свою локальную копию реестра микросервисов, без обращения к другим серверам в кластере.

Это позволяет избежать неконсистентности данных и снизить вероятность ошибок при обнаружении и использовании микросервисов в системе. Кроме того, режим самосохранения позволяет быстро восстановить работу системы после временных сбоев в сети или на серверах.

Однако, в режиме самосохранения может возникнуть проблема "изоляции", когда некоторые микросервисы не будут доступны для других серверов в кластере, что может привести к снижению производительности системы и ухудшению ее работоспособности. Поэтому, режим самосохранения следует использовать с осторожностью и только в случае необходимости.

В целом, режим самосохранения является важной функцией Eureka Server для обеспечения надежности и устойчивости работы системы регистрации и обнаружения микросервисов в распределенных системах.