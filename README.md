# minima_0.103.30

Маленький гайд для тех, кто по разным причинам не хочет использовать докер, а использует сервис, но хочет иметь последнюю версию minima.jar, 0.103.30 , которая внутри докера. (( Которую почему то не хотят выкладывать в гитхаб )).

Итак, по порядку. Все делаем из под пользователя minima

1. Останавливаем запущенный сервис minima.

``` 
sudo systemctl stop minima_9001 
```

2. Запускаем под пользователем minima докер контейнер минимы из Docker Hub  ((все как в офф мане))

```
docker run -d -e minima_mdspassword=123 -e minima_server=true -v ~/minimadocker9001:/home/minima/data -p 9001-9004:9001-9004 --restart unless-stopped --name minima9001 minimaglobal/minima:latest
```

Ждем, пока все барахло из докера скачается, и запустится.

```
Unable to find image 'minimaglobal/minima:latest' locally
latest: Pulling from minimaglobal/minima
f3ef4ff62e0d: Pull complete 
706b9b9c1c44: Pull complete 
76205aac4d5a: Pull complete 
88e84e3d0c37: Pull complete 
4f4fb700ef54: Pull complete 
b80861a70b3b: Pull complete 
33729990092a: Pull complete 
2feb956fd8a7: Pull complete 
bdbfef7f097f: Pull complete 
9fd8748eac75: Pull complete 
a68bf6ab2c46: Pull complete 
fa2a1ee2cfe8: Pull complete 
Digest: sha256:78e22b911d1cf03d84c5f53c51314b3536244b6cb1f6a06024f754d291c98991
Status: Downloaded newer image for minimaglobal/minima:latest
6e9826b7885102a07a82226ab7886b97f169b50e79bbc4f3e57757ff7fc7bd63
```

3. Проверяем, что контейнер запущен 

```
docker ps -a
```

```
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                                           NAMES
6e9826b78851   minimaglobal/minima:latest   "java -jar minima/mi…"   3 minutes ago   Up 3 minutes   0.0.0.0:9001-9004->9001-9004/tcp, :::9001-9004->9001-9004/tcp   minima9001
```

4. Копируем из докера нужный нам файл minima.jar

```
docker cp minima9001:/usr/local/minima/minima.jar $HOME/minima.jar
```

5. Останавливаем , и удаляем докер минимы. Больше он не нужен

```
docker kill minima9001
```

```
docker rm minima9001
```

6. Перезапускаем сервис минимы

```
sudo systemctl restart minima_9001
```

6. Проверяем версию

```
curl -s 127.0.0.1:9005/status | jq .response.version
```

```
"0.103.30"
```



P.S. 
Для тех, кому лениво крутить все эти докеры, просто скачиваем 

```
wget https://github.com/SNSMLN/minima_0.103.30/raw/main/minima.jar
```
