# Guia de Deploy em VPS Contabo

Este guia detalha o processo de deploy de uma aplicação em uma VPS Contabo rodando **Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-105-generic x86_64)** com **8GB de RAM**.

## 1. Pré-requisitos e verificação do ambiente

1. Conecte-se à VPS por SSH:

```bash
ssh root@dou.zebook.tech
```

2. Verifique versão do sistema e recursos:

```bash
lsb_release -a
free -h
uname -a
```

Confirme que o resultado corresponde à versão e kernel indicados.

3. Certifique-se de que Portainer e Traefik estão instalados e em execução. Verifique com `docker ps`.

## 2. Estrutura dos arquivos

Crie um diretório para o projeto em `/opt/app` e um arquivo de configuração `.env` com as informações SMTP e URLs.

```
/opt/app
├── docker-compose.yml
├── .env
└── src/              # Seu código da aplicação
```
Exemplos desses arquivos estão disponíveis neste diretório.
**docker-compose.yml** conterá a definição dos serviços e integração com Traefik.

**.env** guardará as variáveis sensíveis, como mostrado na próxima seção.

## 3. Conteúdo do arquivo `.env`

```ini
APP_DOMAIN=dou.z
VPS_HOSTNAME=dou.zebook.tech
SMTP_USER=admin@zebook.tech
SMTP_SERVER=smtp.hostinger.com
SMTP_PASS=eydbvyu7
SMTP_PORT=465
NETWORK=ZeBookNet
```

## 4. Exemplo de `docker-compose.yml`

```yaml
version: '3.8'
services:
  app:
    build: ./src
    container_name: minha-aplicacao
    env_file: .env
    networks:
      - ${NETWORK}
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.app.rule=Host(`${APP_DOMAIN}`)"
      - "traefik.http.routers.app.entrypoints=websecure"
      - "traefik.http.routers.app.tls.certresolver=letsencrypt"

networks:
  ${NETWORK}:
    external: true
```

Altere o bloco `build: ./src` de acordo com a localização do seu Dockerfile.

## 5. Passo a passo de instalação

1. **Copie o código da aplicação** para `/opt/app/src`.
2. **Crie o `.env`** com os valores fornecidos.
3. **Crie o `docker-compose.yml`** conforme o exemplo acima.
4. **Inicie os serviços**:

```bash
cd /opt/app
docker compose up -d
```

O docker iniciará a aplicação conectada à rede `ZeBookNet` e exposta pelo Traefik.

## 6. Integração com Portainer e Traefik

- **Portainer**: após subir o `docker compose`, acesse o Portainer pela URL `https://dou.zebook.tech:9443`. Na seção *Stacks*, importe ou crie um stack utilizando o mesmo `docker-compose.yml`.
- **Traefik**: certifique-se de que existe a rede `ZeBookNet` criada por `docker network create ZeBookNet`. Traefik deve estar configurado para usá-la. As labels no `docker-compose.yml` registrarão o roteamento para `dou.z`.

## 7. Validação final e testes

1. Verifique se os contêineres estão saudáveis:

```bash
docker ps
```

2. Acesse `https://dou.z` em um navegador para confirmar que o certificado TLS e o roteamento estão corretos.
3. Teste envio de e-mail via SMTP usando o usuário `admin@zebook.tech`. Se houver falhas, verifique logs do contêiner com `docker logs minha-aplicacao`.

## 8. Resolução de problemas comuns

- **Porta ocupada**: use `lsof -i :PORTA` para descobrir o processo e liberar a porta.
- **Problemas com Traefik**: cheque o arquivo de logs em `/var/log/traefik/` para identificar erros de roteamento ou certificados.
- **E-mail não enviado**: confirme as credenciais no `.env` e se a porta 465 está aberta no firewall da VPS.

