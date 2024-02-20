# Define a imagem base
FROM betrybe/puppetter:1.0

# Define o diretório de trabalho
WORKDIR /app/tests

# Adiciona o arquivo node_modules.tar.gz à imagem
ADD node_modules.tar.gz /app/tests

# Copia todos os arquivos da pasta tests para a imagem
COPY . /app/tests

# Define o comando de inicialização padrão
ENTRYPOINT ["npm"]
CMD ["test"]