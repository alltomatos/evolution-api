# Evolution API – Guia Rápido do Swagger

Este documento foi feito para quem nunca usou Swagger (OpenAPI) e quer testar todos os endpoints da Evolution API sem precisar escrever uma única linha de código.

---

## 1. O que é Swagger?
Swagger é uma interface visual que permite explorar, testar e aprender cada rota da API usando apenas o navegador. Tudo o que você precisa é que a aplicação esteja em execução.

---

## 2. Como acessar o Swagger

1. **Inicie a aplicação** (Docker ou `npm run dev`, conforme seu ambiente).
2. Abra o navegador e digite:
   
   ```text
   http://localhost:<PORT>/docs
   ```
   
   *Substitua `<PORT>` pela porta configurada no arquivo `.env` (por padrão `3000`).*
3. A página do Swagger UI será carregada com a lista de todos os endpoints.

> Caso a página não abra, verifique se a variável `SERVER.DISABLE_DOCS` está como `false` no arquivo de configuração.

---

## 3. Navegando pelos endpoints

A documentação está organizada por **tags** (categorias):

| Tag | Descrição |
|-----|-----------|
| Instance Controller | Criação, conexão e gerenciamento de instâncias. |
| Send Message Controller | Envio de mensagens (texto, mídia, áudios, etc.). |
| Chat Controller | Recursos de chat, contatos, perfil e presença. |
| Group Controller | Criação e administração de grupos. |
| Label Controller | Manipulação de labels. |
| Settings | Configurações da instância. |
| Webhook / Websocket / RabbitMQ / SQS | Integrações de eventos. |
| Typebot / Chatwoot / Chama AI | Integrações externas. |
| JWT | Rotas para renovação de token JWT. |

Clique em uma tag para expandir todos os seus endpoints.

---

## 4. Autenticação
Dependendo da configuração do seu servidor, você precisará de **API Key** ou **Bearer JWT**.

### 4.1 API Key
1. Clique no botão **Authorize** (no topo direito).
2. No campo **apikey**, digite sua chave (ex.: `123456`).
3. Clique em **Authorize** e depois em **Close**.

### 4.2 Bearer Token (JWT)
1. Gere ou renove o token usando o endpoint `/instance/refreshToken/`.
2. Clique em **Authorize**.
3. No campo **bearerAuth**, informe:
   
   ```text
   Bearer <SEU_TOKEN>
   ```
4. Clique em **Authorize** e depois em **Close**.

> Você pode usar ambos os métodos ao mesmo tempo se necessário.

---

## 5. Testando um endpoint (Exemplo prático)

### 5.1 Criar uma instância
1. Expanda **Instance Controller**.
2. Clique em `POST /instance/create`.
3. Clique em **Try it out**.
4. Preencha o **Request body** (exemplo):
   ```json
   {
     "instanceName": "meuTeste",
     "qrcode": true
   }
   ```
5. Clique em **Execute**.
6. Role para baixo em **Responses** para ver o código QR em base64 e informações da instância.

### 5.2 Enviar mensagem de texto
1. Expanda **Send Message Controller**.
2. Selecione `POST /message/sendText/{instanceName}`.
3. Clique em **Try it out**.
4. No campo **instanceName** digite `meuTeste`.
5. No **Request body** informe:
   ```json
   {
     "number": "5511999999999",
     "textMessage": {
       "text": "Olá, mundo!"
     }
   }
   ```
6. Clique em **Execute** e confira a resposta.

> Dica: use `Ctrl + F` na página para encontrar rapidamente um endpoint pelo nome.

---

## 6. Exemplos rápidos de Payload

| Endpoint | Exemplo de body |
|----------|-----------------|
| `/chat/updateMessage/{instanceName}` | <details><summary>Mostrar</summary><pre>{
  "number": "5511999999999",
  "text": "Nova mensagem editada",
  "key": {
    "id": "ABCD1234",
    "remoteJid": "5511999999999@s.whatsapp.net",
    "fromMe": true
  }
}</pre></details> |
| `/group/create/{instanceName}` | <details><summary>Mostrar</summary><pre>{
  "subject": "Grupo de Teste",
  "description": "Descrição opcional",
  "participants": ["5511999999999", "5511888888888"]
}</pre></details> |
| `/settings/set/{instanceName}` | <details><summary>Mostrar</summary><pre>{
  "reject_call": true,
  "msg_call": "Estamos ocupados no momento",
  "groups_ignore": false,
  "always_online": false,
  "read_messages": true,
  "read_status": false,
  "sync_full_history": false
}</pre></details> |

(Lembre-se de substituir números de telefone, IDs e nomes conforme sua necessidade.)

---

## 7. Dúvidas frequentes

**1. Meu endpoint retorna 401 – Unauthorized.**  
Verifique se você configurou a `apikey` ou o token JWT em **Authorize**.

**2. O campo de resposta está vazio.**  
Algumas rotas realizam ações assíncronas; confira logs no terminal da aplicação.

**3. O QR Code não aparece.**  
Assegure-se de ter passado `"qrcode": true` na criação da instância e espere alguns segundos após clicar em **Execute**.

---

## 8. Próximos passos

• Explore os demais endpoints seguindo o mesmo fluxo: **expandir ➜ Try it out ➜ preencher ➜ Execute**.  
• Consulte a [documentação oficial](https://doc.evolution-api.com) para detalhes avançados.

Bom proveito! :rocket: