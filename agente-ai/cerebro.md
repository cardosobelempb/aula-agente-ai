## cérebro

### Merge
- Mode = Apped
- Number of Inputs = 4

### AI Agent
- Agent = Tools Agent
- Source for Prompt (User Message) = Define Below
- Prompt (User Message) = {{ (() => $json.text ? $json.text : $json.content)() || $json.mensagem}}

### prompt
- System Message = Hoje é  {{ $now.toString() }} e você é a Letty, assistente que está recepcionando os clientes no whatsapp da clínica de estética “Encha de Beleza”. O seu papel é entender o que os cliente buscam e realizar agendamentos de consultas. Atue de maneira humanizada e profissional.

Horário de funcionamento: 
08:00 as 21:00 de segunda  a sexta

Endereço: Shopping Conjunto Nacional – Sala 1024 – Brasília DF

Caso o cliente queira dicas sobre receitas caseiras para beleza consute a função "Vector Store Tool"

Passo a passo do atendimento:
1) descubra o motivo do atendimento
2) faça sugestão pra uma avaliação gratuita
3) se não quiser fazer a avaliação ofereça marcar um procedimento
4) colete os dados como, nome completo, idade e horário disponível para realizar o agendamento


Passo a passo do atendimento:
1) Pergunte o horário que o cliente quer fazer o procedimento. Use a ferramenta “Buscar Disponibilidade” para verificar se o horário está vago. Se não houver disponibilidade peça outro horário. Exemplo:
Situação 1: “Qual horário você gostaria de fazer seu {{nome_do_procedimento}}?”
Situação 2: “Esse horário está ocupado, poderia me informar outro horário?”
Situação 3: “Ótimo, esse horário está vago, posso marcar?”
2) Se o horário estiver vago e o cliente permitir você vai precisar coletar os dados do cliente para agendamento: {{nome}} e {{email}}
Exemplo:
“Poderia me informar o seu nome completo para eu completar o agendamento?”
3) Quando tiver o nome do procedimento, o email do cliente e nome completo acione a ferramenta “Criar Consulta1”
4) Quando o agendamento for finalizado informe-o do sucesso e com os dados de “eventId”, data e horário e nome do procedimento
Exemplo:

Situação de sucesso: “ A sua consulta foi marcada e será na nossa clínica no Conjunto nacional sala 1024.
Horário: {{horário_marcado}}
Procedimento: {{nome_do_procedimento}}
Caso queira cancelar ou modificar seu evendo informe esse código {{eventId}}”

Situação de erro: “Não conseguimos realizar seu agendamento, posso ternar novamente maracar um {{nome_do_procedimento}} no dia {{horário_marcado}} utilizando o e-mail {{email}}? Esses dados estão corretos?”


Regras obrigatórias:  
- Nunca finalize uma consulta sem validar a disponibilidade antes.  
- Sempre peça o e-mail do cliente para concluir o agendamento.  
- Sempre envie o ID da consulta ao final do agendamento.  
- Nunca sugira horários sem antes consultar a agenda.  
- Se a data informada for no passado ou no mesmo dia, informe que apenas datas futuras são aceitas.  

### Code

```bash
function limparMensagem(texto) {
  if (typeof texto !== 'string') {
    return '';
  }

  // Função para remover metadados e dados técnicos
  function removerMetadataTecnico(str) {
    return str
      // Remove objetos e chaves de metadados específicos
      .replace(/"response_metadata"\s*:\s*{[^}]*}/g, '')  // Remove "response_metadata"
      .replace(/"additional_kwargs"\s*:\s*{[^}]*}/g, '')  // Remove "additional_kwargs"
      .replace(/"tool_calls"\s*:\s*\[\s*\]/g, '')  // Remove "tool_calls" vazio
      .replace(/"invalid_tool_calls"\s*:\s*\[\s*\]/g, '')  // Remove "invalid_tool_calls" vazio
      .replace(/"type"\s*:\s*"(ai|human)"/g, '')  // Remove os tipos "ai" e "human"
      .replace(/"data"\s*:\s*{[^}]*}/g, '')  // Remove a chave "data"
      // Remove objetos JSON em excesso ou vazios
      .replace(/,\s*{[^}]*}/g, '') // Remove objetos soltos
      .replace(/,\s*\[\s*\]/g, '') // Remove arrays vazios
      .replace(/\s*:\s*null/g, '') // Remove valores null
      .replace(/\s*:\s*\[\]/g, '') // Remove arrays vazios
      .replace(/\s*:\s*{}/g, '') // Remove objetos vazios
      // Ajusta espaços desnecessários
      .replace(/\s+/g, ' ')
      .replace(/^\s+|\s+$/g, '');  // Remove espaços no início e fim
  }

  // Função para limpar caracteres especiais
  function limparCaracteresEspeciais(str) {
    return str
      .replace(/\\\\[rnt]/g, ' ')  // Limpa sequências de escape
      .replace(/\\\\\"/g, '')  // Remove as aspas escapadas
      .replace(/\\\\\\\\/g, '')  // Remove barras invertidas
      .replace(/[\x00-\x1F\x7F-\x9F]/g, '')  // Remove caracteres de controle
      .replace(/\"+/g, '')  // Remove aspas extras
      .replace(/[{}[\]]/g, '')  // Remove chaves e colchetes extras
      .trim();
  }

  // Função para extrair e limpar a mensagem principal
  function extrairMensagemPrincipal(str) {
    // Divide em frases, removendo pontuação extra
    const frases = str.split(/(?<=[.!?])\s+/);

    return frases
      .map(frase => frase.trim())
      .filter(frase => {
        // Remove frases que ainda têm metadados ou são irrelevantes
        return frase.length > 0 &&
          !frase.includes('tool_calls') &&
          !frase.includes('invalid_tool_calls') &&
          !frase.match(/^[:\s\[\]{}]+$/);
      })
      .join(' ');
  }

  // Processo de limpeza e extração
  let resultado = texto;

  // Passo 1: Remove metadados e chaves indesejadas
  resultado = removerMetadataTecnico(resultado);

  // Passo 2: Extrai a mensagem relevante, ignorando o que não é necessário
  resultado = extrairMensagemPrincipal(resultado);

  // Passo 3: Remove caracteres especiais e formatação indesejada
  resultado = limparCaracteresEspeciais(resultado);

  // Limpeza final para remover espaços extras
  resultado = resultado
    .replace(/\s+/g, ' ')
    .replace(/^[\",\s]+|[\",\s]+$/g, '')
    .trim();

  return resultado;
}

function processarMensagens(items) {
  return items.map(item => {
    try {
      if (!item?.json?.mensagem) {
        return item;
      }

      let mensagem = item.json.mensagem;

      // Se for objeto, converte para string
      if (typeof mensagem === 'object') {
        try {
          mensagem = JSON.stringify(mensagem);
        } catch (e) {
          console.error('Erro ao converter objeto para string:', e);
          return item;
        }
      }

      // Aplica a limpeza
      const mensagemLimpa = limparMensagem(mensagem);

      // Atualiza apenas se houver conteúdo significativo
      if (mensagemLimpa && mensagemLimpa.length > 0) {
        item.json.mensagem = mensagemLimpa;
      }

      return { json: item.json };
    } catch (error) {
      console.error('Erro ao processar item:', error);
      return item;
    }
  });
}

// Execução principal
try {
  const items = $input.all();
  return processarMensagens(items);
} catch (error) {
  console.error('Erro na execução:', error);
  throw error;
}

```

### OpenAI3
```bash
Credential to connect with = OpenAi Api
Resource = Image
Operation = Analyse Imagem
Model = GPT-4O-MINI
Text Input = Descreva essa imagem, oque tem nela?
Input Data Field Name = data
Simplify Output = true
Options none
```

### Redis Chat Memory
- Credential to connect with: Redis Api
- Session ID = Define below
```bash
fx: Key = 558399126797_memory
{{ $('Normaliza').item.json.message.chat_id }}_memory
***************************************************************
Key Type: Automatic
```
- Session Time To Live = 10000
- Context Window Length = 5/20
