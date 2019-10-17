A. Running the Eureka Service as a single Docker Container

    1. Build Docker Image
        > mvn clean package docker:build -Dmaven.test.skip=true -f ./pom.xml

    2. Docker-compose config
        eureka1:
            image: itmuch/microservice-discovery-eureka:0.0.1-SNAPSHOT
            ports:
              - "8761:8761"
            networks:
              cloud:
                aliases:
                  - eureka
            environment:
              - EUREKA_HOST_NAME=eureka1
              - EUREKA_REPLICAS=1
              - SELF_PRESERVATION=false
        
        eureka2:
            image: itmuch/microservice-discovery-eureka:0.0.1-SNAPSHOT
            ports :
                - "8762:8761"
            networks:
                cloud:
                    aliases:
                        - eureka
            environment:
                - EUREKA_URL_LIST=http://eureka1:8761/eureka/

        
B. Running the Eureka Service and deploy to k8s
    
    1. Build Docker Image
        > mvn clean package docker:build -Dmaven.test.skip=true -f ./pom-k8s.xml
        
    2. To enable each eureka pod in k8s knows other eureka pod, we need issue a StatefulSet for eureka 
        the statefulset sample please check eureka-statefulset.yaml   