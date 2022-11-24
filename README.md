# Dockerizando aplicação de chat feita com ReactJS e NodeJS

Integrantes do grupo: André Canton, Arthur Vassoler, Bruno Abido, Guilherme Bortolotti e Lucas Reginato

# Resumo

Nesse trabalho vamos dockerizar a aplicação **[Realtime Chat Application](https://github.com/adrianhajdin/project_chat_application)** desenvolvida pelo **[Adrian Hajdin](https://github.com/adrianhajdin)** dono do canal [JavaScript Mastery](https://www.youtube.com/@javascriptmastery).  O projeto consiste em um chat em tempo real usando React no front-end, e NodeJS + Socket.io no back-end.

Em comparação com o repositório da aplicação original, há as seguintes mudanças:

- Endpoint da API consumida no front dinâmica utilizando o hostname na porta 5000
    
    ```jsx
    const ENDPOINT = `http://${window.location.hostname}:5000`;
    ```
    
- Dockerfile do front-end (client)
- Dockerfile do back-end (server)
- docker-compose.yml
- Utilização do node ao invés do nodemon para a inicialização do servidor

## Dockerfile client

```docker
FROM node:16

WORKDIR /app

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

## Dockerfile server

```docker
FROM node:16

WORKDIR /app

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 5000

CMD [ "npm", "start" ]
```

## docker-compose.yml

```jsx
services:
  server:
    build:
      context: ./server/
    command: npm start
    ports:
      - "5000:5000"
  client:
    build:
      context: ./client/
    command: npm start
    depends_on:
      - server
    ports:
      - "3000:3000"
```

# Instruções para rodar o projeto

## Requisitos

- Ter git instalado e configurado na máquina
- Ter Docker e Docker-compose instalados e configurados na máquina

1. Clonar repositório
    
    ```jsx
    git clone https://github.com/dedecanton/docker-work.git
    ```
    
2. Abrir o projeto no terminal
3. Na raíz do projeto rodar o seguinte comando para fazer a build
    
    ```jsx
    docker-compose build
    ```
    
4. Ainda na raiz do projeto, rodar o seguinte comando para startar a aplicação
    
    ```jsx
    docker-compose up
    ```
    

Com isso, a aplicação front-end estará rodando na porta 3000 do seu localhost, e o back-end na porta 5000
