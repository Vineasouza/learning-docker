# **Commands**

### Rodar uma imagem do Ubuntu no container

- `docker run -it ubuntu`

### Rodar uma imagem do Node no container

- `docker run -it node`

### Listar todos os container ativos

- `docker ps`

### Listar todos os container do computador

- `docker ps -a`

### Flag -d mantém o container rodando em background

- `docker run -d <image>`

### Parar um container que esta sendo rodado em segundo plano

- `docker stop <container-id>`

### Expor uma porta para um container rodar uma imagem

- `docker run -p <port>:<port> <image>`

### Reiniciar um container ja existente

- `docker start <container-id>`

### Iniciar container com um nome especifico

- `docker run --name <name> <image>`

### Mostrar os logs do container

- `docker logs <container-name>`

### Remover container

- `docker rm <container-name>`

### Buildar uma imagem criada do Dockerfile

- `docker build <dir>`

### Listar as imagens existentes

- `docker image ls`

### Nomear uma imagem

- `docker build -t <name-image> <dir>`

### Apagar uma imagem

- `docker image rm <image-id>` ou `docker rmi <image-id>`

### Apagar tudo que não está sendo utilizado

- `docker system prune`
