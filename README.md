# chatbot-plano-diretor-natal
#Chatbot para Consulta ao Plano Diretor Municipal de Natal

## 📌 Visão Geral

Este projeto implementa um chatbot interativo capaz de responder perguntas utilizando como base de conhecimento o arquivo PDF do Plano Diretor Municipal de **Natal-RN**. Inspirado no desafio "Criando um Chatbot Baseado em Conteúdo de PDFs" da DIO, este projeto adapta a proposta original para uma aplicação prática e de alto valor no campo da Engenharia Civil e Urbanismo.

O objetivo é facilitar o acesso e a consulta a informações específicas dentro de um documento extenso e complexo, utilizando Inteligência Artificial Generativa, embeddings e busca vetorial através dos serviços da Microsoft Azure.

## 🍦 Motivação

O desafio original da DIO focava na análise de artigos científicos para um TCC. Como Engenheiro Civil, vi a oportunidade de aplicar essa tecnologia a um documento fundamental para o planejamento urbano e a prática da engenharia na cidade de Natal: o Plano Diretor. A dificuldade em localizar rapidamente informações específicas e realizar interpretações diretas neste tipo de documento motivou a criação deste chatbot como uma ferramenta de auxílio inteligente.

## 📊 Como Funciona (Ilustrado pelos Prints)

O processo, utilizando as ferramentas do Azure AI Studio, envolve as seguintes etapas principais:

1.  **Configuração da Infraestrutura de IA no Azure (Passos 1-4):**
    * Criação e configuração do projeto (`proj-dio-dois`) no Azure AI Studio (Print 1).
    * Implantação dos modelos de IA necessários através do Azure OpenAI Service:
        * Um Modelo de Embedding como o `text-embedding-3-large` para a vetorização do texto (Print 3, 4).
        * Um Modelo de Linguagem Grande (LLM) como o `DeepSeek-V3` (Print 2, 4) e/ou acesso a modelos como `gpt-4o` para a geração de respostas.
![Captura de tela 2025-05-04 110827](https://github.com/user-attachments/assets/210e4d4d-9d3a-4c57-b754-578a1d4c37bb)
![Captura de tela 2025-05-04 111739](https://github.com/user-attachments/assets/5347199e-0869-4dc2-a710-b31a691c0328)
![Captura de tela 2025-05-04 112436](https://github.com/user-attachments/assets/022d4000-7c8b-429c-bc2d-6e4c0067a5ff)
![Captura de tela 2025-05-04 112943](https://github.com/user-attachments/assets/8ffcbb66-97c1-45d0-965a-45e4e92ff359)


2.  **Carregamento, Processamento e Indexação do Plano Diretor (Passos 5, 6 e 7):**
    * Através da funcionalidade "Adicionar seus dados" no Azure AI Studio, o arquivo PDF do Plano Diretor de Natal (`PLANO_DIRETOR_COMPILADO.V3.pdf`) é carregado (Print 5).
![Captura de tela 2025-05-04 113042](https://github.com/user-attachments/assets/bffa4761-e725-4ecb-8564-a0d1ca7f5ffc)

    * As configurações do índice são definidas no Azure AI Search (utilizando um serviço específico como `diobusca` e um índice vetorial como `serene-endive-7nhtg1qlkg`) (Print 6).
    * O Azure AI Search, integrado ao Azure AI Studio, processa o PDF. O Print 7 ("Status de ingestão") mostra a etapa de **"Quebra e agrupamento - Em andamento"**, indicando que o texto está sendo segmentado (chunking).
    * Durante ![Captura de tela 2025-05-04 113703](https://github.com/user-attachments/assets/320684f0-2feb-47d4-a614-44301eb29ec5)
este processo, os embeddings são gerados para cada segmento usando `text-embedding-3-large` e armazenados no índice vetorial do Azure AI Search.

3.  **Configuração e Preparação para Interação via Chat (Passo 7):**
    * No ambiente de chat/playground do Azure AI Studio, o LLM `gpt-4o` é selecionado para interagir com os dados indexados (Print 7).
    * Um **prompt de sistema** é definido para guiar o comportamento do assistente (ex: "Você é um assistente de IA que ajuda as pessoas a encontrar informações.").
    * A fonte de dados personalizada (o Plano Diretor de Natal indexado no Azure AI Search) é explicitamente vinculada a esta configuração de chat.
![Captura de tela 2025-05-04 114701](https://github.com/user-attachments/assets/b0393890-272b-4891-a51c-9184dbe3f6ad)

4.  **Interação via Chat e Geração de Resposta (Passo 8):**
    * No ambiente de chat do Azure AI Studio (Playground), o usuário faz uma pergunta específica sobre o Plano Diretor de Natal. Por exemplo: *"Olá, tenho um terreno de 200m2 no bairro de capim macio, quanto eu posso construir nele?"* (Print 8 - Pergunta do usuário).
    * O sistema então:
        * Vetoriza a pergunta do usuário usando o modelo `text-embedding-3-large`.
        * Realiza uma busca semântica no índice do Azure AI Search para encontrar os trechos mais relevantes do Plano Diretor de Natal.
        * Envia esses trechos relevantes, juntamente com a pergunta original e o prompt de sistema, para o modelo LLM (`gpt-4o`).

5.  **Apresentação da Resposta Contextualizada (Passo 8):**
    * O LLM (`gpt-4o`) gera uma resposta detalhada e contextualizada, baseada nas informações extraídas do Plano Diretor. No exemplo do Print 8, o chatbot informa sobre a taxa de ocupação máxima para um terreno em Capim Macio, calcula a área permitida para os primeiros pavimentos (ex: *"até 160m² (80% de 200m²) no subsolo, térreo e segundo pavimento"*), e orienta sobre como proceder para pavimentos superiores e onde buscar mais informações (SEMURB), citando referências do documento.
    * A resposta é exibida na interface de chat para o usuário.

## ✨ Insights e Aprendizados

* **IA Aplicada com Precisão:** Este projeto é um exemplo prático de como a IA Generativa, combinada com RAG (Retrieval Augmented Generation) e o ecossistema Azure (Azure AI Studio, Azure OpenAI Service, Azure AI Search), pode ser aplicada para resolver problemas reais em domínios específicos. A capacidade de responder a uma pergunta como *"quanto posso construir em um terreno de 200m2 em Capim Macio?"* com base no Plano Diretor de Natal, incluindo cálculos e referências, demonstra o poder desta abordagem.
* **Democratização da Informação:** Ferramentas como esta podem facilitar enormemente o acesso e a compreensão de documentos técnicos complexos, como um Plano Diretor, por engenheiros, arquitetos, corretores e até mesmo pelo cidadão comum interessado em construir ou entender as regulamentações de sua cidade.
* **Ecossistema Azure para IA:** A plataforma Azure oferece um conjunto robusto e integrado de ferramentas para implantar modelos de embedding, LLMs, criar índices de busca vetorial (com o Azure AI Search) e configurar interfaces de chat com dados próprios ("chat with your data"), agilizando significativamente o desenvolvimento de soluções de IA personalizadas e prontas para produção.
* **Importância do RAG:** A técnica de Retrieval Augmented Generation foi fundamental para que o LLM gerasse respostas baseadas no conteúdo específico do Plano Diretor de Natal, evitando alucinações e fornecendo informações precisas e referenciadas.
* **Desafios e Qualidade:** A qualidade da extração de texto do PDF, a estratégia de chunking (divisão do texto), a relevância dos resultados da busca vetorial e a capacidade do LLM de sintetizar a informação são cruciais para a precisão e utilidade das respostas. A escolha de modelos como `text-embedding-3-large` para embeddings e `gpt-4o` para geração de respostas contribui para a alta qualidade.
* **Potencial de Ferramenta Profissional:** Um chatbot como este tem o potencial de se tornar uma ferramenta valiosa para profissionais da área de construção civil e urbanismo, economizando tempo na consulta de regulamentações e auxiliando na tomada de decisões preliminares.

## 🚀 Possibilidades Futuras

* Suporte a múltiplos documentos relacionados (Leis Complementares, Código de Obras de Natal, Mapas de Zoneamento em formatos legíveis).
* Refinamento da busca vetorial com técnicas avançadas e melhoria contínua dos prompts.
* Integração com outras fontes de dados, como sistemas de informação geográfica (GIS) para consultas baseadas em localização exata.
* Criação de uma interface web standalone para o chatbot, fora do ambiente do Azure AI Studio, para acesso facilitado por um público mais amplo.
* Avaliação e monitoramento contínuo da precisão das respostas e satisfação do usuário.

## 🛠️ Tecnologias Utilizadas

* **Plataforma Cloud:** Microsoft Azure
* **Serviços de IA Principais:** Azure AI Studio, Azure OpenAI Service
* **Modelo de Linguagem (LLM) para Chat com Dados:** `gpt-4o` (configurado no playground do Azure AI Studio para interagir com os dados indexados).
    * *(Nota: `DeepSeek-V3` também foi explorado/implantado, demonstrando flexibilidade na escolha de modelos).*
* **Modelo de Embedding:** `text-embedding-3-large` (Implantado via Azure OpenAI Service, utilizado pelo Azure AI Search).
* **Indexação e Busca Vetorial:** Azure AI Search (utilizando um serviço específico como `diobusca` para hospedar o índice vetorial do Plano Diretor).
* **Processamento de PDF (Chunking, etc.):** Realizado nativamente pela funcionalidade "Adicionar seus dados" do Azure AI Studio.
* **Interface do Chatbot (Demonstração):** Playground do Azure AI Studio.
