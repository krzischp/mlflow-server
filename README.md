```bash
docker-compose up
```

```bash
.\mc alias set minio http://localhost:9000 admin senhasenha
.\mc mb minio/umbucketqualquer
```

# Subindo uma API HTTP para servir o modelo registrado

Depois de ter adicionado o serviço classificador-produtos ao arquivo compose.yml, executar o comando:
```bash
docker-compose up --build
```

## DEBOGAR o serviço API classificador-produtos

Adicionar ao Dockerfile da pasta classificador-produtos:

```bash
# TEST
ENV MLFLOW_TRACKING_URI=http://mlflow:5000
ENTRYPOINT ["bash","./wait-for-it.sh", "mlflow:5000","-t", "90", "--", "mlflow", "models", "serve", "-m", "models:/classificador-produtos/1", "-p", "8080", "-h", "0.0.0.0"]
```

Executar:
```bash
cd classificador-produtos
docker build -t mlflow-server-api .
docker run -p 80:80 --rm --name mlflow-server-api-container --network mlflow-server_public mlflow-server-api
```
Para conseguir entrar no container e executar comandas dentro:
```bash
docker run -d -p 80:80 --rm --name mlflow-server-api-container --network mlflow-server_public mlflow-server-api tail -f /dev/null
docker exec -it mlflow-server-api-container bash
```
Depois, essa linha de comanda deveria dar um erro por causa de conda env create que é "killed"...
```bash
bash ./wait-for-it.sh mlflow:5000 -t 90 -- mlflow models serve -m 'models:/classificador-produtos/1' -p 8080 -h 0.0.0.0
```


