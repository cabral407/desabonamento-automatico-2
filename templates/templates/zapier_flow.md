# Fluxo Zapier: Desabonamento Automático

1. **Trigger:** Outlook - Novo e-mail com "has:unsubscribe"
2. **GPT (OpenAI):** Extrai o link de desinscrição do corpo do e-mail
3. **Webhook Zapier:** Envia POST para o servidor Flask
4. **Servidor Flask:** Executa o clique com Selenium no link extraído
5. **Google Sheets:** Armazena data, e-mail, link e status
6. *(Opcional)* Telegram: Notifica via bot
7. *(Opcional)* Notion: Atualiza painel com a entrada
