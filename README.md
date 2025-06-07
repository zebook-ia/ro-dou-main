![banner](docs/img/banner.png)
# Ro-DOU

[![CI Tests](https://github.com/gestaogovbr/Ro-dou/actions/workflows/ci-tests.yml/badge.svg)](https://github.com/gestaogovbr/Ro-dou/actions/workflows/ci-tests.yml)

O Ro-DOU é uma ferramenta que realiza clipping do Diário Oficial da União (D.O.U.) e de diários municipais por meio do [Querido Diário](https://docs.queridodiario.ok.org.br/pt-br/latest/). A solução permite receber notificações por e‑mail, Slack, Discord ou outros canais sempre que publicações contiverem as palavras‑chave definidas pelo usuário.

Para detalhes de funcionamento e uso, acesse a documentação completa em <https://gestaogovbr.github.io/Ro-dou/>.

O Ro-DOU foi desenvolvido pela Secretaria de Gestão e Inovação do [Ministério da Gestão e da Inovação em Serviços Públicos](https://www.gov.br/gestao/pt-br).

## Sobre o código

O projeto é escrito em Python e utiliza o Apache Airflow para gerar DAGs dinâmicas a partir de arquivos YAML localizados em `dag_confs`. Os módulos no diretório `src` implementam buscadores para o D.O.U. e outros diários oficiais, além de integrações para envio de notificações por e‑mail ou Slack.

Este repositório contém apenas o README original como referência. O código-fonte completo está disponível no repositório oficial: <https://github.com/gestaogovbr/Ro-dou>.
