## Tool/Functions

### Calculator
### Google Calendar Deletar Consulta
```bash
Credential to connect with = Google Calendar API
Tool Description = Set Automatically
Resource = Event
Operation = delete
Calendar = n8n
Event ID 
{{ $fromAI("Event_ID", "O identificador único do evento a ser deletado", "string") }}
Options = none
```
### Buscar Disponibilidade
```bash
Credential to connect with = Google Calendar API
Tool Description = Set Automatically
Resource = Calendar
Operation = Availability
Calendar = n8n
Start Time
{{ $fromAI("Initital_DateTime", "Data e hora inicial da consulta Ex.: 2024-10-17 00:00:00") }}
End Time
{{ $fromAI("Final_DateTime", "Data e hora final da consulta Ex.: 2024-10-17 00:00:00") }}
Options = none
```

### Google Calendar Criar Consulta1
```bash
Credential to connect with = Google Calendar API
Tool Description = Set Automatically
Resource = Event
Operation = create
Calendar = n8n
Start
{{ $fromAI("Start_Time","Horário inicial do evento ex.:2024-10-08 00:00:00") }}
End
{{ $fromAI("End_Time","Horário final do evento ex.:2024-10-08 00:01:00") }}
Use Default Reminders = true
Additional Fields Attendees
{{ $fromAI ("e-mail", "o email do cliente") }}
Use Default Reminders Send Updates = All
Use Default Reminders Summary
Consulta - {{ $fromAI("Nome") }}
Options = none
```

### Vector Store Question Answer Tool
```bash
Description of Data = none
Limit = 4
```
1. Pinecone Vector Store
```bash
Credential to connect with = Pinecone Api
Operation Mode = Retrieve Documents (As vector store for chiin/tools)
Pinecone Index = 
Rerank Results = false
```

2. OpenAI Chat Model
```bash
Credential to connect with = OpenAi Api
Model = GPT-4O-MINI
Options none
```

### HTTP Request saida
- Enviar Mensagem WhatsApp Lead6
```bash
Method = POST
URL
{{ $('Normaliza').item.json.instance.Server_url }}/message/sendText/{{ $('Normaliza').item.json.instance.Name }}
Authentication = none
Send Query Parameters = false
Send Headers = true
Header Parameters
name = apikey
value = {{ $('Normaliza').item.json.instance.Apikey }}
Send Body = true
Body Content Type = JSON
Specify Body = Unsing JSON
JSON
# {
#   "number": "{{ $('Normaliza').item.json.message.chat_id }}",
#   "text": "{{ $json.output.replace(/\n/g, '\\n').replace(/\"/g, '\\"').trim() }}"
# }
Options none
```