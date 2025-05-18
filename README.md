# SistemaAprendizagemAgentes
Sistema interativo de aprendizado com agentes de IA, desenvolvido com Google ADK e modelos Gemini, gerando aulas adaptadas ao perfil do usuário e respondendo dúvidas baseadas em busca.


## Sobre o Projeto

Este projeto foi desenvolvido como parte da **Imersão IA da Alura em parceria com o Google**. O objetivo principal é criar um sistema de aprendizado interativo e personalizado que utiliza múltiplos agentes de Inteligência Artificial para guiar o usuário no estudo de um tópico de sua escolha.

O sistema simula um tutor virtual que se adapta ao perfil do aluno, busca informações atualizadas, redige uma aula didática, fornece material de revisão e permite uma sessão interativa de perguntas e respostas para fixar o conhecimento. Todo o histórico da sessão pode ser salvo localmente.

## Funcionalidades

* **Perfilamento do Aluno:** Entende o nível de conhecimento, público-alvo e estilo de aprendizado do usuário.
* **Busca de Conteúdo:** Utiliza a ferramenta `google_search` para encontrar informações relevantes e recentes sobre o tópico.
* **Redação de Aula:** Gera um texto de aula completo e adaptado ao perfil do aluno e aos resultados da busca.
* **Material de Revisão:** Cria um resumo conciso dos pontos-chave e lista as fontes consultadas.
* **Sessão Tira Dúvidas:** Permite ao aluno fazer perguntas específicas sobre o tópico, que são respondidas buscando informações e adaptando a resposta ao perfil.
* **Salvamento de Histórico:** Salva o conteúdo completo da sessão de estudo em um arquivo local (.md) no ambiente de execução.

## Como Rodar o Projeto

O projeto é ideal para ser executado no **Google Colab**, aproveitando o ambiente Python com as bibliotecas e acesso à API Key.

1.  **Abra o Notebook no Google Colab:** Faça o upload do arquivo `.ipynb` (que você baixou do Colab) para o seu Google Drive e abra-o com o Google Colab, OU faça upload direto para o ambiente Colab.
2.  **Configure a API Key do Google Gemini:**
    * Você precisará de uma API Key do Google Gemini (disponível no Google AI Studio).
    * No Google Colab, vá para o ícone de chave (🔑) na barra lateral esquerda ("Secrets").
    * Clique em "+ New secret".
    * Crie um Secret chamado `GOOGLE_API_KEY` e cole sua chave API nele. Marque a opção para conceder acesso ao notebook.
3.  **Execute as Células:** Rode as células do notebook sequencialmente.
    * A primeira célula instala as bibliotecas e configura a API Key.
    * As células seguintes definem as funções auxiliares e os agentes.
    * A célula final contém o fluxo principal do programa.
4.  **Interação com o Usuário:** O programa irá parar em vários momentos para pedir a sua entrada (tópico, respostas do perfil, dúvidas, decisão de salvar) através de prompts `input()`. Siga as instruções no output.
5.  **Salvamento Local:** Ao final da sessão de dúvidas, você será perguntado se deseja salvar o histórico em um arquivo local (`.md`) no ambiente do Colab. Se confirmar, o arquivo será criado na pasta `/content/` do Colab, e você poderá baixá-lo pela barra lateral de arquivos (ícone de pasta).

## Arquitetura do Projeto: Agentes de IA

O sistema é construído com base em uma orquestração de múltiplos agentes especializados, cada um com uma função específica no fluxo de aprendizado:

1.  **Agente Recepcionista:** Inicia a interação, entende o perfil do aluno através de perguntas e sumariza as respostas.
2.  **Agente Buscador:** Utiliza a ferramenta `google_search` para pesquisar informações relevantes e recentes sobre o tópico, gerando pontos-chave e fontes.
3.  **Agente Redator:** Redige o texto completo da aula, adaptando a linguagem, profundidade e exemplos ao perfil do aluno e usando os resultados da busca como base.
4.  **Agente Review:** Cria um resumo rápido em tópicos do material da aula e lista as fontes consultadas para revisão.
5.  **Agente Dúvidas:** Entra em um loop interativo onde o aluno pode fazer perguntas específicas. Utiliza `google _search` para buscar respostas e as adapta ao perfil do aluno.
6.  **Agente Salvador Local:** Pergunta ao aluno se deseja salvar o histórico completo da sessão em um arquivo local.

## Tecnologias Utilizadas

* Python
* Google Agent Development Kit (ADK)
* Modelos Google Gemini (`gemini-2.0-flash` utilizado por padrão)
* Ferramenta `google_search`
* Bibliotecas Python: `requests` (para verificar links), `os`, `datetime`, `textwrap`, `re`.

## Desafios e Aprendizados

O desenvolvimento deste projeto trouxe aprendizados valiosos, especialmente na orquestração de múltiplos agentes e na interação com frameworks como o Google ADK:

* **Orquestração e Fluxo de Dados:** Planejar como a saída de um agente se torna a entrada para o próximo foi um desafio interessante.
* **Interação Multi-Turno:** Implementar loops interativos (como a sessão de dúvidas) que envolvem a comunicação contínua entre o usuário e um agente via `call_agent` exigiu cuidado no gerenciamento do estado da conversa (neste caso, simulado passando o histórico na entrada).
* **Framework ADK e Contexto:** Um desafio técnico significativo foi encontrar um erro `KeyError` (`Context variable not found: `TÓPICO`.`) relacionado ao processamento interno de instruções pelo ADK, que esperava variáveis de contexto específicas (`TÓPICO`) que não conseguimos fornecer de forma robusta através da nossa função auxiliar `call_agent`. Isso indica a complexidade em controlar totalmente o ambiente de execução interno do framework.
* **Autenticação de API (Inicial):** A tentativa inicial de integrar com Google Docs via API OAuth 2.0 apresentou a complexidade de configurar credenciais no Google Cloud Platform e implementar o fluxo de autorização no Colab, o que demonstrou ser um tópico mais avançado e demandaria mais tempo do que o disponível para o prazo.

## Sugestões de Melhorias Futuras

Este projeto pode ser expandido de diversas formas:

* **Integração Completa com Google Docs/Drive:** Implementar a funcionalidade de salvar o histórico diretamente em um documento Google Docs ou arquivo no Google Drive usando as APIs relevantes.
* **Questionário Interativo Avançado:** Desenvolver o agente de questionário com lógica de avaliação de respostas, feedback mais detalhado e adaptação das próximas perguntas com base na performance do aluno.
* **Interface Gráfica (UI):** Criar uma interface web simples (usando Gradio, Streamlit ou Flask) para tornar a interação mais amigável do que os prompts de texto do Colab.
* **Tipos de Conteúdo:** Expandir a geração de conteúdo para incluir outros formatos (tabelas, listas mais ricas, exemplos de código interativos).
* **Feedback Baseado na Dúvida:** O agente de dúvidas poderia não apenas responder, mas também sugerir tópicos relacionados ou materiais de aprofundamento com base nas perguntas mais frequentes do aluno.
* **Análise do Histórico:** Implementar a funcionalidade de analisar o histórico de sessões salvas para identificar áreas de dificuldade do aluno ao longo do tempo.
