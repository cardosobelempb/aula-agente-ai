# SendList

```bash
evoBaseUrl = {{https://evo.surb.com.br}}
evoInstance = NomeInstancia
evoApiKey = 16265CDF8E45-440C-B522-F17369678083
```
## Method 
```
- POST / {{evoBaseUrl}}/message/sendList/{{evoInstance}}

```

## Header
- apiKey = evoApiKey

# Paramts Advance
- Timeout (s) = 10s

## Body

```bash
{
    "number": "{{remoteJid}}",
    "title": "",
    "description": "Prazer {{nome}} Escolha uma opção:",
    "buttonText": "ESCOLHA UMA OPÇÃO",
    "footerText": "",
    "sections": [
        {
          "title": "",
          "rows": [
                {
                    "title": "Atendimento",
                    "description": "Falar com o setor de atendimento",
                    "rowId": "001"
                },
                {
                    "title": "Comercial",
                    "description": "Falar com setor comercial",
                    "rowId": "002"
                },
                {
                    "title": "Suporte",
                    "description": "Falar com setor de suporte",
                    "rowId": "003"
                },
                {
                    "title": "Financeiro",
                    "description": "Falar com setor financeiro",
                    "rowId": "004"
                }
            ]
        } 
    ]  
}
```