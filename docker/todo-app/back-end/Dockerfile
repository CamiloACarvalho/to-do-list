# Define a imagem base
FROM node:16-alpine

# Expondo a porta 3001
EXPOSE 3001

# Define o diretório de trabalho
WORKDIR /app/back-end

# Adiciona o arquivo node_modules.tar.gz à imagem
ADD node_modules.tar.gz /app/back-end

# Copia todos os arquivos da pasta back-end para a imagem
COPY . /app/back-end

# Define o comando de inicialização padrão
ENTRYPOINT ["npm"]
CMD ["start"]

