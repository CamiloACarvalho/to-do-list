# :construction: README em construção ! :construction:
<!--
1. Crie um container em modo interativo, sem rodá-lo, nomeando-o como 01container e utilizando a imagem alpine na versão 3.12

# Para criar um container novo: "docker contair create -it"
# Para nomear o container criado: "--name (nome do container)"
# Para que esse container tenha uma determinada imagem com versão "(nome da imagem):(versão)"

docker container create -it --name 01container alpine:3.12

----------------------------------------------------------

 2. Inicie o container 01containe

# Para dar início ao container "docker start (nome do container)"

docker start 01container

------------------------------------------------------------

3. Liste os containers filtrando pelo nome 01container 

# Para filtrar um determinado container "docker --filter (alguns comandos)"
# Alguns comando são:
  # "status=runing" - filtra apenas os que estão rodando
  # "name=(nome do container desejado)" - filtras determinado container pelo nome
  # Pode também aplicar os dois filtros --filter "status=runing" --filter "name=(nome)"
# Caso queira listar esse container usar "ps" depois do docker: "docker ps..."

docker --filter "name=01container"

------------------------------------------------------------

 4. Execute o comando cat /etc/os-release no container 01container sem se acoplar a ele 

# docker é usado para interagir  com o "docker"
# exec é usado para executar comandos dentro dos containers que já estão em execução
# nome do container
# comando que deseja executar dentro do container (cat /etc/os-release)

docker exec 01container cat /etc/os-release

---------------------------------------------------------------

 5. Remova o container 01container

# Para remover um container usamos o comando "rm"
# Caso não dê certo, utilizamos o -f que indicar "forçar"
# Por fim o nome do container que desejamos remover

docker rm -f 01container

---------------------------------------------------------------

 6. Faça o download da imagem nginx com a versão 1.21.3-alpine sem criar ou rodar um container

# docker é usado para interagir com o Docker
# image é um subcomando do docker que é utilizado para trabalhar com as imagens do docker
# pull, assim como no git, o comando pull serve para (puxa) fazer o download para a máquina
# nginx nome da imagem desejada
# 1.21.3-alpine indica a versão da imagem

docker image pull nginx:1.21.3-alpine

Resumindo: esse comando solicita ao Docker para baixar a imagem do Nginx na versão 1.21.3, baseada no Alpine Linux, do registro de contêiner padrão (Docker Hub) para sua máquina. Isso permitirá que você tenha essa imagem disponível localmente para criar e executar contêineres a partir dela.

---------------------------------------------------------------

 7. Rode um novo container com a imagem nginx com a versão 1.21.3-alpine em segundo plano nomeando-o como 02images e mapeando sua porta padrão de acesso para porta 3000 do sistema hospedeiro 

# run é um subcomando para criar e iniciar novos conteiners a partir de uma imagem docker
# -d é uma opção do comando "docker run" e indica que é para iniciar o conteiner em segundo plano (requisito do exercício)
# -p é outro comando de "docker run" que serve para mapear a porta do conteiner para uma porta no host
# 3000:80 a porta 80 é onde estamos mapeando (monitorando), que é a porta onde o servidor Nginx está sendo executado e 3000 é o localhost que desejamos acessar o servidor do Nginx. Seria algo mais ou menos assim: 3000 é a porta que vamos criar na nossa máquina para acessar o servidor da Nginx que é a porta 80. 

docker run -d -p 3000:80 --name 02images nginx:1.21.3-alpine

-------------------------------------------------------------

 8. Pare o container 02images que está em andamento

# stop como o próprio nome diz, é para parar conteiners que estão sendo executados
# 02images é um argumento do docker stop que pode indicar o ID ou o nome, neste caso indica o nome do conteiner que deseja parar

docker stop 02images

---------------------------------------------------------------

 9. Gere uma build a partir do Dockerfile do back-end do todo-app nomeando a imagem para todobackend

# docker: É o comando principal da interface de linha de comando do Docker.
# build: Este é um subcomando usado para construir imagens Docker. Ele cria uma imagem Docker a partir de um Dockerfile e de outros arquivos do contexto.
# -t: Esta é uma opção do comando docker build. É usada para adicionar uma tag ou nome à imagem que está sendo construída. Por exemplo, -t todobackend adicionaria a tag todobackend à imagem.
# todobackend: Este é o nome da imagem que você deseja criar.
# ./todo-app/back-end: Este é o diretório onde o Dockerfile e outros arquivos necessários para construir a imagem estão localizados. É o contexto de construção. Quando você executa docker build, o Docker usa os arquivos neste diretório (e em subdiretórios) para construir a imagem Docker.

docker build -t todobackend ./todo-app/back-end

-Dentro do arquivo Dockerfile dentro do diretório back-end tem os seguintes comando:

# Define a imagem base
FROM node:16-alpine

_Esta instrução indica a imagem base que será utilizada como ponto de partida para a construção da nova imagem. Neste caso, node:16-alpine é uma imagem oficial do Node.js na versão 16, baseada no Alpine Linux. Isso significa que o Dockerfile estará construindo uma imagem que utiliza o Node.js como ambiente de execução e o Alpine Linux como sistema operacional base. O Alpine Linux é uma escolha comum para imagens Docker devido ao seu tamanho reduzido._

# Expondo a porta 3001
EXPOSE 3001

_Esta instrução indica a imagem base que será utilizada como ponto de partida para a construção da nova imagem. Neste caso, node:16-alpine é uma imagem oficial do Node.js na versão 16, baseada no Alpine Linux. Isso significa que o Dockerfile estará construindo uma imagem que utiliza o Node.js como ambiente de execução e o Alpine Linux como sistema operacional base. O Alpine Linux é uma escolha comum para imagens Docker devido ao seu tamanho reduzido._

# Define o diretório de trabalho
WORKDIR /app/back-end

_Esta instrução define o diretório de trabalho dentro do contêiner para /app/back-end. Todas as instruções subsequentes serão executadas neste diretório, a menos que especificado de outra forma. Isso é útil para garantir que os comandos ADD, COPY, RUN, entre outros, sejam executados no contexto do diretório especificado._

# Adiciona o arquivo node_modules.tar.gz à imagem
ADD node_modules.tar.gz /app/back-end

_Esta instrução extrai um arquivo node_modules.tar.gz para o diretório /app/back-end dentro do contêiner. Presumivelmente, este arquivo contém as dependências do Node.js necessárias para o aplicativo. Utilizar um arquivo tar.gz pré-compilado pode acelerar o processo de construção, pois evita que o Docker reinstale as dependências do Node.js em cada compilação._

# Copia todos os arquivos da pasta back-end para a imagem
COPY . /app/back-end

_Esta instrução copia todos os arquivos e diretórios do contexto de construção (onde está localizado o Dockerfile) para o diretório /app/back-end dentro do contêiner. Isso inclui todos os arquivos do código-fonte do aplicativo, bem como quaisquer outros arquivos presentes no contexto de construção._

# Define o comando de inicialização padrão
ENTRYPOINT ["npm"]

_Esta instrução define o ponto de entrada padrão para o contêiner. Ela especifica o comando a ser executado quando o contêiner for iniciado. Neste caso, o comando npm é definido como o ponto de entrada. Isso significa que qualquer comando ou argumento passado para o contêiner será executado como se fosse um comando npm._

CMD ["start"]

_Esta instrução fornece argumentos padrão para o ponto de entrada definido anteriormente. Se nenhum comando for especificado quando o contêiner for iniciado, o comando especificado aqui será executado. Neste caso, o comando npm start será executado por padrão quando o contêiner for iniciado._

---------------------------------------------------------------------------------

 10. Gere uma build a partir do Dockerfile do front-end do todo-app nomeando a imagem para todofrontend

# -t: Esta é uma opção do comando docker build. É usada para adicionar uma tag ou nome à imagem que está sendo construída. 
# todofrontend: Este é o nome da imagem que você deseja criar.
# ./todo-app/front-end: Este é o diretório onde o Dockerfile e outros arquivos necessários para construir a imagem estão localizados.

docker build -t todofrontend ./todo-app/front-end/

-Dentro do arquivo Dockerfile dentro do diretório front-end tem os seguintes comando:

FROM node:16-alpine
EXPOSE 3000
WORKDIR /app/front-end
ADD node_modules.tar.gz /app/front-end
COPY . /app/front-end
ENTRYPOINT ["npm"]
CMD ["start"]

----------------------------------------------------------------------------------

 11. Gere uma build a partir do Dockerfile dos testes do todo-app nomeando a imagem para todotests 
 
# -t: Esta é uma opção do comando docker build. É usada para adicionar uma tag ou nome à imagem que está sendo construída. 
# todotests : Este é o nome da imagem que você deseja criar.
# ./todo-app/tests: Este é o diretório onde o Dockerfile e outros arquivos necessários para construir a imagem estão localizados. 
 
docker build -t todotests ./todo-app/tests
 
-Dentro do arquivo Dockerfile dentro do diretório tests tem os seguintes comando:

FROM betrybe/puppetter:1.0
WORKDIR /app/tests
ADD node_modules.tar.gz /app/tests
COPY . /app/tests
ENTRYPOINT ["npm"]
CMD ["test"]

----------------------------------------------------------------------------------------

12. Suba uma orquestração em segundo plano com o docker-compose de forma que backend, frontend e tests consigam se comunicar 

# docker-compose: Este é o comando principal para interagir com o Docker Compose, que é uma ferramenta para definir e executar aplicativos Docker com múltiplos contêineres.
# up: Este é um subcomando do Docker Compose usado para criar e iniciar contêineres para todos os serviços definidos no arquivo docker-compose.yml. Ele inicia os serviços em primeiro plano por padrão, mas pode ser combinado com a opção -d para iniciar os serviços em segundo plano (modo detached).
# -d: Esta opção indica ao Docker Compose para iniciar os serviços em segundo plano, ou seja, em modo detached.


docker-compose up -d

- Dentro do arquivo docker-compose.yml na raiz do diretório Docker temos os seguintes comandos:

# A versão 3 é uma das versões mais recentes e suporta uma série de recursos avançados e melhores práticas de composição de contêineres
# Solicitação do projeto
version: '3'
# Aqui é onde definimos os serviços que irão compor a aplicação.
services:
  # Este é o nome do serviço que estamos definindo. Ele será usado para referenciar este serviço em outros lugares do arquivo.
  # Aqui acabei criando um contianer para o frontend, backend e para os testes.
  todofront:
    # Aqui estamos usando a imagem que criamos no Dockerfile. Isso significa que o Dockerfile deve estar no mesmo diretório que este arquivo.
    # Aqui estamos usando todofrontend porque já foi criado um container com esse nome
    image: todofrontend
    # Aqui estamos mapeando a porta 3000 do contêiner para a porta 3000 do host. Isso significa que podemos acessar o aplicativo em http://localhost:3000.
    ports:
      - "3000:3000"
    # Aqui estamos definindo uma variável de ambiente que será usada pelo aplicativo para acessar o backend.
    # Isso é necessário porque o aplicativo frontend e o backend estão em contêineres separados.
    environment:
    # Já aqui, estamos usando o container todoback que foi o container criado dentro service
      - REACT_APP_API_HOST=todoback
    # Aqui estamos dizendo que este serviço depende do serviço todobackend.
    # Isso significa que o serviço todofrontend só será iniciado depois que o serviço todobackend estiver pronto.
    depends_on:
    # Mesma coisa aqui, o todoback é o nome do container que foi criado dentro do service para o backend
      - todoback

  # Este é o serviço que representa o backend da aplicação.    
  todoback:
    # Aqui estamos usando a imagem que criamos no Dockerfile dentro da pasta do backend.
    image: todobackend
    ports:
      - "3001:3001"

  todotests:
    image: todotests
    # O environment é usado para definir variáveis de ambiente que serão usadas pelo aplicativo.
    environment:
      - FRONT_HOST=todofront
    # Aqui estamos dizendo que este serviço depende do serviço do front-end e do serviço do back-end.
    depends_on:
      - todofront

-->
