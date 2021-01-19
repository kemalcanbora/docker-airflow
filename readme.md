### Dockerfile to Hub
 - sudo docker build -t airflow .
 - sudo docker tag airflow:latest kemalcanbora/docker-airflow:latest
 - sudo docker push kemalcanbora/docker-airflow

### Docker-compose

    version: '3.3'
    services:
        webserver:
            image:  kemalcanbora/docker-airflow:latest
            restart: always
            environment:
                - AIRFLOW__CORE__EXECUTOR=SequentialExecutor
            logging:
                options:
                    max-size: 10m
                    max-file: "3"
            volumes:
                - ./:/usr/local/airflow/dags
            ports:
                - "8080:8080"
            command: webserver
            healthcheck:
                test: ["CMD-SHELL", "[ -f /usr/local/airflow/airflow-webserver.pid ]"]
                interval: 30s
                timeout: 30s
                retries: 3