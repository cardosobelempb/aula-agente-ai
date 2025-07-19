# Session bot Paused

```bash
botPaused = https://evo.surb.com.br/typebot/changeStatus/typebot
apiKey = 16265CDF8E45-440C-B522-F17369678083
```

## Method 
```
- POST / {{baseUrl}}/typebot/changeStatus/{{instance}}

```

## Header
- apiKey

## Body

```
{
  "remoteJid": "{{remoteJid}}",
  "status": "paused" /* opened, paused, closed */
}
```