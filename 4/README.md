## Explorei um Índice no Azure AI Search (UI)

Fui solicitado a ajudar na criação de uma solução de mineração de conhecimento para facilitar a busca de insights sobre as experiências dos clientes. Decidi criar um índice no Azure AI Search usando dados extraídos de avaliações de clientes.

### Recursos do Azure Necessários

Para a solução do Fourth Coffee, criei os seguintes recursos na minha assinatura do Azure:
- Um recurso do Azure AI Search, que gerencia a indexação e a consulta.
- Um recurso de serviços de IA do Azure, que fornece serviços de IA para habilidades que a solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.
- Uma conta de armazenamento com contêineres de blobs, que armazena documentos brutos e outras coleções de tabelas, objetos ou arquivos.

## Criei um Recurso do Azure AI Search

1. Entrei no portal do Azure.
2. Cliquei no botão + Criar um recurso, pesquisei por Azure AI Search.
3. Criei um Recurso Azure AI Search com as seguintes configurações:
   - Assinatura: Minha subscrição Azure.
   - Grupo de recursos: Selecionei ou criei um grupo de recursos com um nome exclusivo.
   - Nome do serviço: Um nome exclusivo.
   - Localização: Escolhi qualquer região disponível.
   - Preços nível: Básico.

4. Selecionei Review + create e depois de ver a resposta Validation Success, selecionei Create.
5. Após a conclusão da implantação, selecionei Ir para o recurso. Na página de visão geral do Azure AI Search, pude adicionar índices, importar dados e pesquisar índices criados.

## Criei um Recurso de Serviços de IA do Azure

Provisionei um recurso de serviços de IA do Azure que estava no mesmo local que meu recurso do Azure AI Search. Minha solução de pesquisa usou esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

1. Retornei à página inicial do portal do Azure. Cliquei no botão + Criar um recurso e pesquisei por Serviços de IA do Azure.
2. Fui levado a uma página para criar um recurso de serviços de IA do Azure e configurei-o com as seguintes configurações:
   - Assinatura: Minha subscrição Azure.
   - Grupo de recursos: O mesmo grupo de recursos que meu recurso do Azure AI Search.
   - Região: O mesmo local do recurso do Azure AI Search.
   - Nome: Um único nome.
   - Preços nível: Padrão S0.

3. Selecionei Revisar + criar. Depois de ver a resposta Validation Passed, selecionei Create.
4. Aguardei a conclusão da implantação e visualizei os detalhes da implantação.

## Criei uma Conta de Armazenamento

1. Retornei à página inicial do portal do Azure e selecionei o botão + Criar um recurso.
2. Procurei por conta de armazenamento.
3. Criei um recurso de conta de armazenamento com as seguintes configurações:
   - Assinatura: Minha subscrição Azure.
   - Grupo de recursos: O mesmo grupo de recursos que os recursos do Azure AI Search e dos serviços Azure AI.
   - Nome da conta de armazenamento: Um nome exclusivo.
   - Localização: Escolhi qualquer localização disponível.
   - Padrão de desempenho.
   - Redundância: armazenamento localmente redundante (LRS).

4. Cliquei em Revisar e em Criar. Aguardei a conclusão da implantação e fui para o recurso implantado.
5. Na conta de Armazenamento do Azure que criei, no painel de menu esquerdo, selecionei Configuração (em Configurações).
6. Alterei a configuração de Permitir acesso anônimo de Blob para Habilitado e selecionei Salvar.

## Carreguei Documentos para o Armazenamento Azure

1. No painel do menu esquerdo, selecionei Containers.
2. Selecionei + Contêiner. Um painel do seu lado direito foi aberto.
3. Inseri as seguintes configurações e cliquei em Criar:
   - Nome: Coffee-Reviews.
   - Nível de acesso público: Container (acesso de leitura anônimo para containers e blobs).
   - Avançado: sem alterações.

4. Em uma nova guia do navegador, baixei as avaliações compactadas do café em https://aka.ms/mslearn-coffee-reviews e extraí os arquivos para a pasta de avaliações.
5. No portal do Azure, selecionei o contêiner de avaliações de café. No contêiner, selecionei Carregar.
6. No painel Carregar blob, selecionei Selecionar um arquivo.
7. Na janela do Explorer, selecionei todos os arquivos na pasta de avaliações, selecionei Abrir e, em seguida, selecionei Carregar.
8. Depois que o upload foi concluído, pude fechar o painel Upload blob. Meus documentos estavam agora em meu contêiner de armazenamento de avaliações de café.

## Indexei os Documentos

Após armazenar os documentos, pude usar o Azure AI Search para extrair insights dos documentos. O portal do Azure forneceu um assistente de importação de dados. Com este assistente, pude criar automaticamente um índice e um indexador para fontes de dados suportadas. Usei o assistente para criar um índice e importar meus documentos de pesquisa do armazenamento para o índice do Azure AI Search.

1. No portal do Azure, naveguei até o recurso do Azure AI Search. Na página Visão geral, selecionei Importar dados.

2. Na página Conectar-se aos seus dados, na lista Fonte de Dados, selecionei Azure Blob Storage.

3. Concluí os detalhes do armazenamento de dados com os seguintes valores:
   - Fonte de dados: Azure Blob Storage.
   - Nome da fonte de dados: coffee-customer-data.
   - Dados a extrair: Conteúdo e metadados.
   - Modo de análise: Padrão.
   - Cadeia de conexão: *Escolhi uma conexão existente. Selecionei minha conta de armazenamento, selecionei o contêiner de avaliações de café e cliquei

 em Selecionar.
   - Gerenciamento de identidade de autenticação: Nenhuma.
   - Nome do contêiner: Esta configuração foi preenchida automaticamente depois que escolhi uma conexão existente.
   - Pasta Blob: Deixei em branco.
   - Descrição: Avaliações sobre Fourth Coffee Shops.

4. Selecionei Próximo: Adicionar habilidades cognitivas (opcional).

5. Na seção Anexar Serviços Cognitivos, selecionei meu recurso de serviços Azure AI.

6. Na seção Adicionar enriquecimentos, fiz as seguintes configurações:
   - Alterei o nome da qualificação para coffee-skillset.
   - Marquei a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content.
   - Certifiquei-me de que o campo Dados de origem estava configurado como merged_content.
   - Alterei o nível de granularidade de enriquecimento para Páginas (blocos de 5.000 caracteres).
   - Não selecionei Habilitar enriquecimento incremental.
   - Selecionei os seguintes campos enriquecidos:
     | Habilidade Cognitiva | Nome do Campo |
     | --- | --- |
     | Extrair nomes de localização | Localizações |
     | Extrair frases-chave | Frases-chave |
     | Detectar sentimento | Sentimento |
     | Gerar Tag de imagens | Tags de imagem |
     | Gerar legendas de imagens | Legendas de imagem |

7. Em Salvar enriquecimentos em um armazenamento de conhecimento, selecionei:
   - Projeções de Imagem
   - Documentos
   - Páginas
   - Frases-chave
   - Entidades
   - Detalhes da imagem
   - Referências da imagem.
   - Se apareceu um aviso solicitando uma cadeia de conexão de conta de armazenamento, selecionei Escolher uma conexão existente, escolhi a conta de armazenamento que criei anteriormente.

8. Selecionei + Container para criar um novo contêiner chamado armazenamento de conhecimento com o nível de privacidade definido como Privado e selecionei Criar.

9. Selecionei o contêiner de armazenamento de conhecimento e cliquei em Selecionar na parte inferior da tela.

10. Selecionei projeções de blob do Azure: Documento. Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento. Não mudei o nome do contêiner.

11. Selecionei Próximo: Personalizar índice de destino. Alterei o nome do índice para coffee-index.

12. Certifiquei-me de que a chave estava configurada como metadata_storage_path. Deixei o nome do sugeridor em branco e o modo de pesquisa preenchido automaticamente.

13. Revisei as configurações padrão dos campos de índice. Selecionei filtrável para todos os campos que já estavam selecionados por padrão.

14. Selecionei Próximo: Criar um indexador.

15. Alterei o nome do indexador para coffee-indexer.

16. Deixei a programação definida como Once.

17. Expanda as opções avançadas. Certifiquei-me de que a opção Base-64 Encode Keys estava selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

18. Selecionei Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador foi executado automaticamente e executou o pipeline de indexação, que:
    - Extraiu os campos de metadados do documento e o conteúdo da fonte de dados.
    - Executou o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
    - Mapeou os campos extraídos para o índice.

19. Na metade inferior da página Visão geral do recurso Azure AI Search, selecionei a guia Indexadores. Esta guia mostrou o indexador de café recém-criado. Esperei um minuto e selecionei &orarr; Atualizar até que o Status indicasse sucesso.

20. Selecionei o nome do indexador para ver mais detalhes.

## Consultei o Índice

Usei o Search Explorer para escrever e testar consultas. O explorador de pesquisa é uma ferramenta incorporada no portal do Azure que oferece uma maneira fácil de validar a qualidade do meu índice de pesquisa. Usei o Search Explorer para escrever consultas e revisar resultados em JSON.

1. Na página Visão geral do serviço de pesquisa, selecionei Explorador de pesquisa na parte superior da tela.

2. Observei como o índice selecionado era o índice de café que criei.

3. No campo Cadeia de consulta, inseri search=*&$count=true e selecionei Pesquisar. A consulta de pesquisa retornou todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count.

4. Agora vamos filtrar por localização. Insira search=locations:'Chicago' no campo String de consulta e selecione Pesquisar. A consulta pesquisou todos os documentos no índice e filtrou revisões com localização em Chicago.

5. Agora vamos filtrar por sentimento. Insira search=sentiment:'negative' no campo String de consulta e selecione Pesquisar. A consulta pesquisou todos os documentos no índice e filtrou revisões com sentimento negativo.

6. Observação: Os resultados são classificados por @search.score. Esta é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão próximos os resultados correspondem à consulta fornecida.

7. Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa para entender a causa da revisão.

## Revisei o Armazenamento de Conhecimento

Vi o poder do armazenamento de conhecimento em ação. Ao executar o assistente Importar dados, também criei um armazenamento de conhecimento. Dentro do armazenamento de conhecimento, encontrei os dados enriquecidos extraídos pelas habilidades de IA que persistem na forma de projeções e tabelas.

1. No portal do Azure, naveguei de volta para a minha conta de armazenamento do Azure.

2. No painel do menu esquerdo, selecionei Containers. Selecionei o contê

iner de armazenamento de conhecimento.

3. Dentro do contêiner de armazenamento de conhecimento, observei várias pastas. Cada pasta representa uma projeção diferente, como Projeção de imagem, Documentos, Páginas, Frases-chave, Entidades, Detalhes da imagem e Referências da imagem.

4. Explorei a pasta Documentos e abri um dos arquivos JSON. O arquivo continha dados de metadados do documento e o conteúdo extraído do documento. Esses dados foram enriquecidos com insights de IA, como frases-chave, localizações, sentimento e tags de imagem.

5. No campo merged_content, vi o conteúdo do documento combinado, incluindo texto extraído de imagens com OCR.

## Conclusão

Ao seguir essas etapas, criei um índice no Azure AI Search usando dados extraídos de avaliações de clientes. Usei habilidades de IA para enriquecer os dados com insights adicionais, como localização, frases-chave, sentimento e tags de imagem. Além disso, explorei o armazenamento de conhecimento para ver como os dados enriquecidos são armazenados e acessíveis para análise adicional. Este é um passo crucial para a solução de mineração de conhecimento para o Fourth Coffee, fornecendo uma base sólida para a busca de insights sobre as experiências dos clientes.