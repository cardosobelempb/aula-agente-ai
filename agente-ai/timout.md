## timout
### webhook
- Webhook URLs: test/poduction
- HTTP Method: POST
- Path: go
- Authentication: none
- Respond: Immediately
- Options: none

### Normalizes
- Mode: Manual Mapping
- Fields to Set
```bash
# indentificação da mensagem que chega dentro do n8n
String: message.message_id = 3A29B1638B86BC6E7422
{{ $json.body.data.key.id || '' }}
***************************************************************
# indentificação do numero do telefone do cliente que chega dentro do n8n
String: message.chat_id = 558399126797
{{ $json.body.data.key.remoteJid.split('@')[0] || '' }}
***************************************************************
# indentificação todos os tipos de mensagem que chega dentro do n8n
String: message.content_type = text
{{ $('Webhook').item.json.body.data.message.extendedTextMessage ? 'text' : '' }}
{{ $('Webhook').item.json.body.data.message.conversation ? 'text' : '' }}
{{ $('Webhook').item.json.body.data.message.audioMessage ? 'audio' : '' }}
{{ $('Webhook').item.json.body.data.message.imageMessage ? 'image' : '' }}
***************************************************************
# indentificação o conteudo da mensagem que chega dentro do n8n
String: message.content = Olá
{{ $('Webhook').item.json.body.data.message.extendedTextMessage?.text || '' }}
{{ $('Webhook').item.json.body.data.message.imageMessage?.caption || '' }}
{{ $('Webhook').item.json.body.data.message.conversation || '' }}
***************************************************************
# indentificação a data que url chegou a mensagem que chega dentro do n8n
String: message.content_current_date = 
{{ $('Webhook').item.json.body.data.messageTimestamp.toDateTime('s').toISO() }}
***************************************************************
# indentificação se e um audio ou uma imagem da mensagem que chega dentro do n8n
String: message.content_url = 
{{ $('Webhook').item.json.body.data.message.audioMessage?.url || '' }}
{{ $('Webhook').item.json.body.data.message.imageMessage?.url || '' }}
***************************************************************
# indentificação se e uma mendagem de entrada(incoming) o saida(outcoming) da mensagem que chega dentro do n8n
String: message.event = incoming
{{ $('Webhook').item.json.body.data.key.fromMe ? 'outcoming' : 'incoming' }}
***************************************************************
# indentificação o nome da instacia da mensagem que chega dentro do n8n
String: instance.name = surb-letty
{{ $json.body.instance }}
***************************************************************
# indentificação a apikey do n8n da mensagem que chega dentro do n8n
String: instance.apikey = 47372FC06FED-4AD7-80F8-4A0DE3391C73
{{ $json.body.apikey }}
***************************************************************
# indentificação a url da evolution da mensagem que chega dentro do n8n
String: instance.evo_url = https://evo.surb.com.br
{{ $json.body.evo_url }}
***************************************************************
# indentificação a apikey do elevenlabs da mensagem que chega dentro do n8n
String: apiKey_elevenlabs = sk_24c5816f10ebf863b5c81f7d07949ab30e94360e9
{{ $json.body.data.key.id || '' }}

```

### Origem Shwite
- Mode: Roles
- Routing Rules
```bash
{{ $json.message.event }}
is equal to
outcoming
Rename Output
Output Name = outcoming
***************************************************************
{{ $json.message.event }}
is equal to
incoming
Rename Output
Output Name = incoming
***************************************************************
```

### Get Block Chat Id Redis
- Credential to connect with: Redis Api
- Operation: GET
- Name: block

```bash
fx: Key = 558399126797_timeout
{{ $json.message.chat_id }}_timeout
***************************************************************
Key Type: Automatic
```

### Gera Timeout Redis
- Credential to connect with: Redis Api
- Operation: SET

```bash
fx: Key = 558399126797_timeout
{{ $json.message.chat_id }}_timeout
***************************************************************
Value: true
Key Type: String
Expire: true
TTL: 900
```

### Switch Block
- Mode: Roles
- Routing Rules
```bash
{{ $json.block }}
is empty
Rename Output
Output Name = IA PODE RESPONDER
***************************************************************
{{ $json.block }}
is equal to
true
Rename Output
Output Name = IA NAO PODE RESPONDER
***************************************************************
```

### No Operation