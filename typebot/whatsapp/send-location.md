# SendLocation

```bash
evoBaseUrl = {{https://evo.surb.com.br}}
evoInstance = NomeInstancia
evoApiKey = 16265CDF8E45-440C-B522-F17369678083
```
## Method 
```
- POST / {{evoBaseUrl}}/message/sendLocation/{{evoInstance}}

```

## Header
- apiKey = evoApiKey

# Paramts Advance
- Timeout (s) = 10s

## Body

```bash
{
    "number": "{{remoteJid}}",
    "name": "Salvador Shopping",
    "address": "Av. Tancredo Neves, 3133 - Caminho das Árvores, Salvador - BA, 41820-021",
    "latitude": -12.9778603,
    "longitude": -38.4578313
}
```