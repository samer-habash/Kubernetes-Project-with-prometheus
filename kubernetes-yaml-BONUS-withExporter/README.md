Additional :

1) Added annotations in svc rabbitmq-exporter-service.yaml => NOTE we must add it in order to see it in URL http://prometheus-server/targets
2) Added deployment+service kbudde/rabbitmq-exporter rabbitmq , the setup is separated than Pod rabbitmq
   NOTE: I have tried put the rabbitmq-exporter in the same Pod of rabbitmq and added annotations in the rabbimq-service (But it did not work properly and I keep getting error in prometheus-server as context deadline exceeded .... tried to fix it in the configmap of prometheus without any succes. So I separate them)
3) DockerHub New images : samer1984/app_producer-with-duration , samer1984/app_consumer-with-duration
