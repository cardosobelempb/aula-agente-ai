# SendContact

```bash
evoBaseUrl = {{https://evo.surb.com.br}}
evoInstance = NomeInstancia
evoApiKey = 16265CDF8E45-440C-B522-F17369678083
```
## Method 
```
- POST / {{evoBaseUrl}}/message/sendContact/{{evoInstance}}

```

## Header
- apiKey = evoApiKey

# Paramts Advance
- Timeout (s) = 10s

## Body

```bash
{
    "number": "{{remoteJid}}",
     "contact": [
        {
            "fullName": "Cláudio Cardoso",
            "wuid": "5583999126797",
            "phoneNumber": "+55 83 99912-6797",
            "organization": "Surb SA",
            "email": "contato@surb.com.br",
            "url": "https://surb.com.br"
        }
    ]
}
```