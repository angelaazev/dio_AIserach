### Primeiro passo - criação e configuração do recurso:

## Criar o recurso do Azure AI Search
Entre no portal do Azure (https://portal.azure.com).

Clique no botão + Criar um recurso, pesquise o Azure AI Search e crie um recurso do Azure AI Search com as configurações:

Assinatura: assinatura do Azure.
Grupo de recursos: selecione ou crie um grupo de recursos com um nome exclusivo.
Nome do serviço: um nome exclusivo.
Localização: Escolha qualquer região disponível.
Nível de preços: Básico
Selecione Revisar + criar e, depois de ver a resposta Validação bem-sucedida, selecione Criar.

Após a conclusão da implantação, selecione Ir para o recurso. Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.

## Criar o recurso de IA do Azure
Você precisará provisionar um recurso de serviços de IA do Azure que esteja no mesmo local que seu recurso do Azure AI Search. Sua solução de pesquisa usará esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

Retorne à página inicial do portal do Azure. Clique no botão ＋Criar um recurso e pesquise os serviços de IA do Azure. Selecione criar um plano de serviços de IA do Azure. Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:
Assinatura: assinatura do Azure.
Grupo de recursos: O mesmo grupo de recursos que o seu recurso Azure AI Search.
Região: O mesmo local que o seu recurso Azure AI Search.
Nome: Um nome exclusivo.
Nível de preços: Padrão S0
Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo: Selecionado
Selecione Revisar + criar. Depois de ver a resposta Validação aprovada, selecione Criar.

Aguarde a conclusão da implantação e visualize os detalhes da implantação.

## Crie uma conta de armazenamento
Retorne à página inicial do portal do Azure e selecione o botão + Criar um recurso.

Procure a conta de armazenamento e crie um recurso de conta de armazenamento com as seguintes configurações:
Assinatura: sua assinatura do Azure.
Grupo de recursos: o mesmo grupo de recursos que os recursos do Azure AI Search e dos serviços Azure AI.
Nome da conta de armazenamento: um nome exclusivo.
Localização: Escolha qualquer local disponível.
Padrão de desempenho
Redundância: armazenamento localmente redundante (LRS)
Clique em Revisar e em Criar. Aguarde a conclusão da implantação e vá para o recurso implantado.

Na conta de Armazenamento do Azure que você criou, no painel de menu esquerdo, selecione Configuração (em Configurações).
Altere a configuração de Permitir acesso anônimo de Blob para Habilitado e selecione Salvar.

## Crie um novo container
Na conta de armazenamento criada clique na aba Containers no painel esquerdo e depois em +Container para criar um novo container. Dê um novo para esse novo container e na opção Anonymus acess level selecione: Container (anonymous read access for containers and blobs).

Nesse novo container, faça o upload dos arquivos descompactados presentes no zipped coffee review disponível para download no link: https://aka.ms/mslearn-coffee-reviews

## Indexação dos documentos
No portal do Azure, navegue até o recurso Azure AI Search. Na página Visão geral, selecione Importar dados.
Na página Conectar-se aos seus dados, na lista Fonte de Dados, selecione Armazenamento de Blobs do Azure. Preencha os detalhes do armazenamento de dados com os seguintes valores:
Fonte de dados: Armazenamento de Blobs do Azure
Nome da fonte de dados: coffee-customer-data
Dados a extrair: Conteúdo e metadados
Modo de análise: Padrão
Cadeia de conexão: *Selecione Escolha uma conexão existente. Selecione sua conta de armazenamento, selecione o contêiner de avaliações de café e clique em Selecionar.
Autenticação de identidade gerenciada: Nenhuma
Nome do contêiner: esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente.
Pasta Blob: deixe em branco.
Descrição: Avaliações sobre Fourth Coffee Shops.
Selecione Próximo: Adicionar habilidades cognitivas (opcional).

Na secção Anexar Serviços Cognitivos, selecione o seu recurso de serviços Azure AI.

Na seção Adicionar enriquecimentos:

  Altere o nome da Skillset para coffee-skillset.
  Marque a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content.
  Certifique-se de que o campo Dados de origem esteja definido como merged_content.
  Altere o nível de granularidade de enriquecimento para Páginas (blocos de 5.000 caracteres).
  Não selecione Habilitar enriquecimento incremental
  
Selecione os seguintes campos enriquecidos:

Nome do campo do parâmetro de habilidade cognitiva
Extraia nomes de locais
Extraia frases-chave frases-chave
Detectar sentimento sentimento
Gerar tags a partir de imagens imageTags
Gerar legendas de imagens imageCaption
Em Salvar enriquecimentos em um armazenamento de conhecimento, selecione:
  Projeções de imagem
  Documentos
  Páginas
  Frases chave
  Entidades
  Detalhes da imagem
  Referências de imagem

Selecione projeções de blob do Azure: Documento. Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento. Não altere o nome do contêiner.

Selecione Próximo: Personalizar índice de destino. Altere o nome do índice para índice de café.

Certifique-se de que a chave esteja definida como metadata_storage_path. Deixe o nome do sugeridor em branco e o modo de pesquisa preenchido automaticamente.

Revise as configurações padrão dos campos de índice. Selecione filtrável para todos os campos que já estão selecionados por padrão.

Selecione Próximo: Criar um indexador.

Altere o nome do indexador para indexador de café.

Deixe a programação definida como Uma vez.

Expanda as opções avançadas. Certifique-se de que a opção Base-64 Encode Keys esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

Selecione Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
  Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
  Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
  Mapeia os campos extraídos para o índice.
Na metade inferior da página Visão Geral do recurso Azure AI Search, selecione a guia Indexadores. Esta guia mostra o indexador de café recém-criado. Espere um minuto e selecione &orarr; Atualize até que o Status indique sucesso.

Selecione o nome do indexador para ver mais detalhes.

## Consulte o index:

Na página Visão geral do serviço de pesquisa, selecione Explorador de pesquisa na parte superior da tela.
No campo Cadeia de consulta, insira search=*&$count=true e selecione Pesquisar. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.

Agora vamos filtrar por localização. Insira search=locations:'Chicago' no campo Cadeia de caracteres de consulta e selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra revisões com localização em Chicago.

### Insights e Possibilidades:
Recuperação de Informações: A capacidade de localizar dados relevantes rapidamente é essencial para a experiência e os resultados do usuário final.

Aplicação de Processos de IA: Você pode adicionar habilidades cognitivas para aplicar processos de IA durante a indexação. O uso dos serviços de IA do Azure pode adicionar novas informações e estruturas que são úteis para pesquisa e outros cenários.

Integração com Outros Serviços do Azure: A IA do Azure Search pode se integrar a outros serviços do Azure na forma de indexadores que automatizam a ingestão/recuperação de dados de fontes de dados do Azure e conjuntos de habilidades que incorporam a IA consumível por meio de serviços de IA do Azure.

Aplicativos de Pesquisa Tradicionais e Conversacionais: A Pesquisa de IA do Azure é uma funcionalidade importante nos aplicativos. O mecanismo de Pesquisa de IA do Azure usa a funcionalidade de IA que ajuda os aplicativos a trabalhar de maneira mais humana e fazer associações que vão além da mera correspondência de palavras-chave.

Ferramentas de Criação para Protótipos e Consultas de Índices: Além das APIs, o portal do Azure fornece suporte de administração e gerenciamento de conteúdo, com as ferramentas de criação para protótipos e consultas de seus índices.

Ferramentas de Análise de Imagem e Processamento de Idioma Natural: A IA do Azure Search pode se integrar a outros serviços do Azure, como análise de imagem e processamento de idioma natural, ou a IA personalizada criada por você no Azure Machine Learning ou encapsula no Azure Functions.
