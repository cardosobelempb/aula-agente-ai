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
    "title": "Title Button",
    "description": "Description Button",
    "footer": "Footer Button",
    "buttons": [
        {
            "type": "url",
            "displayText": "Evolution API",
            "url": "http://evolution-api.com"
        }
    ],
    "options": {
      "delay": 1200  
    }
    
}
```