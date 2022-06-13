FROM continuumio/miniconda3:latest

RUN pip install mlflow

ADD . /app
WORKDIR /app

COPY wait-for-it.sh wait-for-it.sh 
RUN chmod +x wait-for-it.sh


# TEST
ENV MLFLOW_TRACKING_URI=http://mlflow:5000
# ENTRYPOINT ["bash","./wait-for-it.sh", "mlflow:5000","-t", "90", "--", "mlflow", "models", "serve", "-m", "models:/classificador-produtos/1", "-p", "8080", "-h", "0.0.0.0"]
