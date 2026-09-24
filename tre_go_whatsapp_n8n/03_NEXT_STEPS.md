# Próximos passos

## Ponto exato em que o chat parou

A infraestrutura da POC foi considerada estável, com a instância `TRE_POC` reconectada, o Webhook associado ao n8n e o fluxo local usando Ollama/`llama3:latest`. A conversa terminou no momento em que foi solicitado ao Copilot criar ou atualizar os arquivos de contexto e fornecer os comandos Git para publicar a consolidação no GitHub.

No plano técnico da aplicação, a próxima melhoria acordada é refinar o System Prompt e validar a qualidade da triagem sem modificar Evolution API, n8n, Webhook, Docker, Redis ou PostgreSQL.

## Instrução operacional da próxima etapa

No n8n (`http://localhost:5678`), abra o workflow de atendimento e atualize somente a instrução do nó que chama o Ollama com as regras SESRE já consolidadas: saudação, classificação da intenção, coleta de nome/cargo/lotação/contato, recusa de assuntos fora de TI e resumo final para GLPI. Salve o workflow.

Depois, execute o teste prático com uma mensagem vaga (por exemplo, `Oi`) e uma demanda de suporte. Confirme que o bot solicita os dados obrigatórios e, com os dados completos, gera o **RESUMO DO CHAMADO**. Registre a resposta observada e qualquer falha antes de planejar a migração futura para `gpt-oss:20b`/API institucional ou a integração com GLPI.

## Restrições para a continuação

- Não recriar a instância WhatsApp sem confirmação.
- Não alterar o endpoint do Webhook ou o modelo sem validar previamente os serviços.
- Não expor credenciais reais; usar placeholders.
- Validar o caminho completo WhatsApp → Evolution API → n8n → Ollama → Evolution API → WhatsApp.
