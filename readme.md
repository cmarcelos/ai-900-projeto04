# Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

:memo: **Note:** Entre no site <https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html> para obter um passo a passo desta atividade. Abaixo vou fazer um resumo do que esta no site.

## Criar um recurso do Azure AI Search

1. Entre no portal do Azure
2. Clique em ***+ criar um novo*** procure por Azure Ai Search.
3. E crie um novo recurso com segue:
- Assiantura: Vai aparecer a sua
- Grupo de recurso: Crie um grupo com um nome de sua preferencia e que seja exclusivo.
- Service name: Crie outro nome (exlusivo)
- Location: Escolha uma região no estados unidos, pois os custos são menores.
- Price / Comandao de preço: Basico
-- aperte o botão ***Revisar + Criar*** se tudo der certo e validação com sucesso.
-- selecione ***Criar***
4. Vá para o recurdo.

## Como criar o recurso de serviço da IA.
1. Vá para <font color="red">Home </font> e então clique em ***create source***
2. IA + Machine Learnig
3. Azure Ai Service
***Criate***
- Assiantura: Vai aparecer a sua
- Grupo de recurso: Crie um grupo com um nome de sua preferencia e que seja exclusivo.
- Location: Escolha uma região no estados unidos, pois os custos são menores.
- Name: Crie outro nome (exlusivo)
- Price tier: Standard S0
- [x] Li e entendi os termos
Apertar o botão ***Review and Create***

## Agora vamos criar uma conta de armazenamento
1. Vá para Storage acconts.
2. ***Criar***
- Assiantura: Vai aparecer a sua
- Grupo de recurso: Crie um grupo com um nome de sua preferencia e que seja exclusivo.
- Storage Account (a escolha do nome e díficil, pois tem que ser um nome único)
- Location: Escolha uma região no estados unidos, pois os custos são menores.
- Performace: Standard
- Redeveclancy: Locally - redundant storage (LRS)

Não precisa ir para as outras abas.
***review*** — &rarr; ***Create***

Como o store account tem seguranças para o laboratório devemos mudar algumas deles então vá para:
Setting — &rarr; Configuração — &rarr; permitir acesso anônimo, alterando para ***Enable*** e ***Save***.

### Fazendo upload de Documentos para o Azure Storage
1. No memu lateral clique em ***Container***  
2. ***Adicionar novo Container*** clicando em +container.
_ Name: coffee-reviews
_ Public access level : container (anonymous read access for containers and blobs)
_Advanced: Não mudar.

3. ***Create***

4. Clicar no container criado.
5. Baixar os documentos em <https://aka.ms/mslearn-coffee-reviews>

6. Voltando ao portal do Azure
7. Clique em ***Upload*** - &rarr; Selecione a pasta onde salvou os documentos e clique em ***Upload***.
8. Com os documentos, vou no meu recurso de busca que criamos em AI Search
9. Clico em importar dados.

## Indexar os documentos
1. No portal do Azure, navegue até o recurso Azure AI Search. Na página Visão geral , selecione Importar dados.

2. ***Conectar-se aos seus dados*** , na lista ***Fonte de Dados*** , selecione ***Azure Blob Storage***
- ***Fonte de dados*** : Armazenamento de Blobs do Azure
- ***Nome da fonte de dados*** : coffee-customer-data
- ***Dados a extrair*** : Conteúdo e metadados
- ***Modo de análise*** : Padrão
- ***Cadeia de conexão*** : *Selecione Escolha uma conexão existente . Selecione sua conta de armazenamento, selecione o contêiner de avaliações de café e clique em Selecionar .
- ***Autenticação de identidade gerenciada*** : Nenhuma
- ***Nome do contêiner*** : esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente .
- ***Pasta Blob*** : deixe em branco .
- ***Descrição*** : Avaliações sobre Fourth Coffee Shops.

3. Então vá em ***Proximo:*** Adicionar abilidades cognitivas

4. Em ***Anexar Serviços Cognitivos***: Coloque o seu recurso Azure AI
5. Em ***Adicionar Enriquecimento***
- ***Nome da qualificação***: coffee-skillset .
-  [x] Habilitar OCR e mesclar todo o texto no campo merged_content .

- Campo Dados de origem: merged_content.
- Nível de granularidade de enriquecimento: Páginas (blocos de 5.000 caracteres) .
- Habilitar enriquecimento incremental <font color="red">Não selecionar</font> 
- Selecione os seguintes campos enriquecidos:
|Habilidade Cognitiva|Nome do campo|
|Extraia nomes de locais|Localizações|
|Extraia frases-chave|frases chave|
|Detectar sentimento|sentimento|
|Gerar tags de imagens|imagemTags|
|Gere legendas de imagens|legenda da imagem|

6. Salvar enriquecimentos em um armazenamento de conhecimento.
- Projeções de imagem
- Documentos
- Páginas
- Frases chave
- Entidades
- Detalhes da imagem
- Referências de imagem

7. ***Projeções de blob do Azure: Documento***não altere o nome.
8. ***Personalizar índice de destino***: coffee-index
9. A ***chave*** deve estar como: metadata_storage_path . o nome do sugeridor:  em branco e o modo de pesquisa preenchido automaticamente.
10. Selecione Filtravel.
11. Proximo &rarr; criar um indexador
12. Nome do indexador : coffee-indexer
13. Programação como Uma vez
14. Em opções avançadas, deixe selecionada a opção ***Base-64 Encode Keys***
15. Selecione Enviar.
17. Volte para recursos do Azure AI Search. 
18. Em Gerenciamento de pesquisa , selecione Indexadores . Selecione coffee-indexer recém-criado . Espere... selecione ↻ Atualize até que o Status indique sucesso.
19. Selecione o indexador.

## Consultar o índice. 
Nota: Esta topico e de autroria da microsoft.
1. Na página Visão geral do serviço de pesquisa , selecione ***Search Explore*** na parte superior da tela.
2. Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a visualização para ***JSON view*** .
No campo do editor de consultas JSON , copie e cole: o codigo:

{
    "search": "*",
    "count": true
}

1. Selecione Pesquisar . A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count . O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.

2. Agora vamos filtrar por localização. No campo do editor de consultas JSON , copie e cole:

{
 "search": "locations:'Chicago'",
 "count": true
}

3. Selecione Pesquisar . A consulta pesquisa todos os documentos no índice e filtra revisões com sentimento negativo. Você deveria ver 1no @odata.count campo.
4. Agora vamos filtrar por sentimento. No campo do editor de consultas JSON , copie e cole:

{
 "search": "sentiment:'negative'",
 "count": true
}

5. Selecione Pesquisar . A consulta pesquisa todos os documentos no índice e filtra revisões com sentimento negativo. Você deveria ver 1no @odata.countcampo.
6. Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa. O que você acha que pode ser a causa da revisão?