FROM python:2.7-alpine
ENV PYTHONPATH /usr/src/app
WORKDIR /usr/src/app
COPY ./ ./
RUN pip install -r requirements.txt
#Expose the metrics in consumer from port 9422
#CMD ["python", "consumer.py", "-p", "9422", "-s", "rabbitmq"]
