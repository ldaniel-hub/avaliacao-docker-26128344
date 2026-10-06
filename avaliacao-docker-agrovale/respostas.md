# Respostas · Avaliação Prática de Docker · Cooperativa AgroVale (Turma A)

Nome: Lucas Rodrigues Daniel
Matrícula: 26128344
Usuário do GitHub: ldaniel-hub
Usuário do Docker Hub: lucasr32

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile ou compose vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
nginx:1.27-alpine, 73.6

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
/usr/share/nginx/html/, docker exec -it teste-portal ls -la /usr/share/nginx/html

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
lucasr32/agrovale-portal:1.0-23128344, https://hub.docker.com/r/lucasr32/agrovale-portal

4. Por que o `docker login` foi feito com um token de acesso e não com a senha da conta?
O repositório precisa estar público para que a avaliação consiga realizar o `docker pull` da imagem e testá-la em um ambiente isolado sem a necessidade de credenciais de acesso

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | WORKDIR /usr/share/nginx | Apontava para a pasta raiz do Nginx e não para a pasta html de servimento de ficheiros. | O Nginx servia a página predefinida "Welcome to nginx!". | Alterado para WORKDIR /usr/share/nginx/html. |
| 2 | COPY (Ausente) | Não existia instrução para copiar os ficheiros da pasta site/ para dentro do container | Os ficheiros do site de manutenção não entravam na imagem | Adicionada a instrução COPY site/ . |
| 3 | EXPOSE (Ausente) | A porta 80 do container não estava documentada nos metadados.    | A imagem não declarava a porta interna exposta | Adicionada a instrução EXPOSE 80. |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
Um mapeia a porta 7044 do host para a porta 80 do container e o outro mapearia a porta 80 do host para a porta 7044 do container.

## Parte 4 · docker-compose.yml

7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?
O valor db é o nome do serviço definido no docker-compose.yml, dentro da rede compartilhada do Docker o servidor DNS interno do Docker resolve o nome do serviço diretamente para o IP do container do banco de dados. Se fosse utilizado o localhost, o WordPress tentaria procurar a base de dados dentro do seu próprio container, gerando erro de conexão.

8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco, como faz sem publicar
a porta? Mostre o comando.
O serviço db não expõe a porta 3306 para o host por motivos de segurança, evitando que o banco de dados fique exposto a acessos não autorizados ou ataques externos. Para consultar o banco diretamente sem publicar a porta, executamos o cliente interativo do MariaDB dentro do próprio container com o comando: docker exec -it avaliacao-docker-agrovale-db-1 mariadb -u agrovale -pavaliacaodb agrovale_blog

## Parte 5 · Persistência

9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou,
e por quê?
docker compose down e docker compose up -d, docker compose down -v. O parâmetro -v remove os volumes nomeados gerenciados pelo Docker, onde estão armazenados o banco de dados MariaDB e os arquivos do WordPress. Sem a flag -v, o docker compose down remove apenas os containers e a rede virtual, preservando os dados gravados nos volumes no hospedeiro.

10. Código de conclusão impresso pelo verificador:
================================================================                                                                                                                
 Verificador · Avaliação Prática de Docker · Turma A                                                                                                                            
================================================================                                                                                                                
 Matrícula 26128344 · portal 8044 · blog 9044 · manutenção 7044                                                                                                                 
                                                                                                                                                                                
A. Arquivos, imagens e Git                                                                                                                                                      
[ OK ] A1 portal/Dockerfile segue os requisitos
[ OK ] A2 imagem manutencao:26128344 corrigida e servindo o aviso
[FALHA] A3 .env fora do Git e .env.example versionado
         -> confira o .gitignore e rode: git ls-files
[ OK ] A4 5+ commits e remoto no GitHub (encontrados: 5)
[ OK ] A5 imagem lucasr32/agrovale-portal:1.0-26128344 pública no Docker Hub

B. Stack em execução
[ OK ] B1 serviços portal, blog e db em execução
[ OK ] B2 portal roda a imagem publicada                                                                                                                                        
[ OK ] B3 portas: portal em 8044 e blog em 9044                                                                                                                                 
[ OK ] B4 db sem porta publicada e com volume nomeado                                                                                                                           
[ OK ] B5 blog com volume nomeado em /var/www/html                                                                                                                              
[ OK ] B6 rede própria compartilhada pelos três serviços                                                                                                                        
[ OK ] B7 política de restart nos três serviços                                                                                                                                 
[ OK ] B8 nenhuma senha escrita direto no docker-compose.yml

C. Conteúdo e persistência
[ OK ] C1 portal mostra seu nome e sua matrícula
[ OK ] C2 WordPress instalado com a matrícula no título do site
[ OK ] C3 post sobreviveu à recriação do blog (post 2026-10-06T01:06:45 · container 2026-10-06T01:19:07)

================================================================
 Resultado: 15/16 verificações
 Ainda há falhas. Corrija e rode de novo.
================================================================