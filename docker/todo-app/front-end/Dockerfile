# Define a imagem base
FROM node:16-alpine

# Expondo a porta 3000
EXPOSE 3000

# Define o diretório de trabalho
WORKDIR /app/front-end

# Adiciona o arquivo node_modules.tar.gz à imagem
ADD node_modules.tar.gz /app/front-end

# Copia todos os arquivos da pasta front-end para a imagem
COPY . /app/front-end

# Define o comando de inicialização padrão
ENTRYPOINT ["npm"]
CMD ["start"]