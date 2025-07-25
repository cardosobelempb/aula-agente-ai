# Aula 2 - O que são Agentes de IA
- Recuroso orientedos a algum LLM **(Large Language Model)** para realizae alguma ação.
- Em vez de tralhar com apenas uma entrada e saida, **o agente raciocina sobre o pque precisa fazer**.
- Pode **utilizar uma ou mais ferramentas (Tools)**, para realizar o trabalho.
- Estas tools podem ser **nós de ação (Sheets, Calendar, Slack e outros)** ou até mesmo **chamar outro workflows**.
- Diferença entre workflow e agente de IA.
  1. **Workflow** = flux repetitivo e continuo.
  2. **Agente de IA**= não tem um fluxo especificoa seguir, cada utilização do agente o fluxo pode ser alterado.
- Algumas vezes conseguimos simplificar um agente em um Workflow.
  
# Aula 3 - A Anatomia do Agente de IA
- AI agente é um nó que precisa de várias configuração para funcionar.
  1. **Trigge**: Como ativar o agente.
  2. **Chat Model**: Modelo para fazer o agente processar as ações.
  3. **Memory**: Memória o agente poder 'lembrar' das interações/conversa.
  4. **Tools**: Uma ou mais ferramnta para o agente desempenhar sua função.
  5. **Output**: Pode ou não ter uma saída de dados, as vezeso agente pode resolver tudo nas tools. 