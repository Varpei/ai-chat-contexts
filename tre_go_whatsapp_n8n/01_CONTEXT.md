# Contexto — TRE Bot POC / SESRE

## Objetivo do projeto

Construir uma Prova de Conceito de atendimento interno via WhatsApp para a Seção de Suporte e Redes (SESRE) do TRE-GO. O bot deve atender servidores da Sede, Anexo II, IALBA, zonas eleitorais e postos avançados, fazer a triagem inicial de demandas de TI e preparar dados para futura abertura de chamados no GLPI e expansão para outras seções.

A estratégia atual é estabilizar e demonstrar a POC localmente. A migração para a infraestrutura institucional de IA do TRE-GO ocorrerá somente após a apresentação e aprovação do projeto.

## Arquitetura validada

- **Evolution API** em Docker, porta `8080`, instância WhatsApp `TRE_POC`.
- **n8n** em Docker, porta `5678`, com Webhook `POST /webhook/receber-whatsapp`.
- **Ollama** local no host, porta `11434`, usando `llama3:latest` durante a POC.
- **PostgreSQL** para persistência da Evolution API.
- **Redis** para cache e estabilidade da sessão da Evolution API.
- Comunicação n8n → Ollama via `host.docker.internal`.
- Fluxo ponta a ponta: WhatsApp → Evolution API → Webhook n8n → Ollama → Evolution API → WhatsApp.

## Tecnologias e artefatos mencionados

Docker Compose, Bash, Python, curl, n8n, Evolution API, WhatsApp/WhatsApp Web, Ollama, PostgreSQL, Redis, JSON de workflows, APIs HTTP/REST, Git/GitHub e GLPI.

Scripts e arquivos citados: `docker-compose.yml`, `conectar.sh`, `conectar-pratico.py`, `definir-webhook.py`, `verificar-webhook.py`, `diagnostico.py`, `auditoria-geral.py`, `workflow-receber-mensagem.json` e documentação em `docs/`.

Credenciais e números reais não devem ser reproduzidos em documentação futura; usar variáveis de ambiente ou placeholders.

## Regras de negócio do atendimento

1. Apresentar-se como assistente virtual oficial da SESRE quando a mensagem for uma saudação ou estiver vaga.
2. Atender exclusivamente demandas de suporte técnico de TI; redirecionar educadamente assuntos fora do escopo.
3. Fazer triagem objetiva, cordial e em português do Brasil.
4. Coletar antes de concluir um chamado: nome completo, cargo/função, setor ou cartório de lotação e contato/ramal quando aplicável.
5. Identificar a intenção/categoria, incluindo criação de conta no domínio Jus/Active Directory, Duo Mobile, permissões, redes, hardware e outros.
6. Quando o assunto envolver Duo Mobile, solicitar o número de celular de forma apropriada.
7. Pedir esclarecimentos para mensagens incompletas, sem inventar dados ou comandos.
8. Ao finalizar, produzir um **RESUMO DO CHAMADO** com `[SOLICITANTE]`, `[LOTAÇÃO]`, `[CATEGORIA]` e `[DESCRIÇÃO DO PROBLEMA]`, deixando o formato pronto para futura integração com GLPI.
9. Preservar o fluxo e a infraestrutura já validados ao alterar apenas o comportamento do prompt.

## Decisões estratégicas

- A POC permanece local para reduzir risco antes da apresentação.
- O modelo local validado para a demonstração é `llama3:latest`; `qwen3-coder` foi descartado por não estar disponível/ser inadequado ao atendimento geral.
- A futura infraestrutura institucional considerada usa a API HTTPS do TRE-GO e o modelo `gpt-oss:20b`, mas não deve ser ativada nesta fase.
- A comunicação entre contêineres não deve usar `localhost` para serviços remotos; os endpoints precisam respeitar a topologia Docker/host validada.
- A sessão WhatsApp deve ser mantida e reconectada, não recriada sem confirmação explícita.
