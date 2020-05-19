FROM python:2.7-alpine
ENV PYTHONPATH /usr/src/app
WORKDIR /usr/src/app
COPY ./ ./
RUN pip install -r requirements.txt
#ENTRYPOINT ["python", "producer.py", "-p", "5672", "-s", "rabbitmq"]
# default is 20 seconds loop messages , you can increase/decrease it by passing -t <duration_in_seconds>
#CMD ["-t", "20"]
