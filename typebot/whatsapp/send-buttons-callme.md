# FakeCall

```bash
evoBaseUrl = {{https://evo.surb.com.br}}
evoInstance = NomeInstancia
evoApiKey = 16265CDF8E45-440C-B522-F17369678083
```
## Method 
```
- POST / {{evoBaseUrl}}/message/sendButtons/{{evoInstance}}

```

## Header
- apiKey = evoApiKey

# Paramts Advance
- Timeout (s) = 10s

## Body

```bash
{
    "number": "{{remoteJid}}",
    "title": "Título do Botão WhatsApp",
    "description": "Descrição do envio do botão WhatsApp",
    "footer": "Footer do botão WhatsApp",
    "buttons": [
        {
            "type": "call",
            "displayText": "Me ligue",
            "phoneNumber": "557193000207"
        }
    ]
}
```