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
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

## Parte 4 · docker-compose.yml

7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?

8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco, como faz sem publicar
   a porta? Mostre o comando.

## Parte 5 · Persistência

9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou,
   e por quê?

10. Código de conclusão impresso pelo verificador:

```
(cole aqui)
```
