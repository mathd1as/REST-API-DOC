# REST API
Alguns serviços oferecidos pelo sistema {nome_do_sistema_geral} são implementados através de microserviços. Eles são escondidos dos usuários, sendo acessado através do {nome_do_sistema_ruby} que funciona como um middleware/proxy? para eles. Essa referência documenta os ponto de acessos desses serviços.
  * `Certificado`
  * `Notificação`
  * `Atas`

## Tabela de Conteúdo

  * [Formato dos Dados](#formato-dos-dados)
  * [Erros padrões da API](#erros-padrões-da-api)
  * [Certificado](#certificado)
    * [Recursos Públicos](#recursos-públicos) 
      * [Pesquisar Certificados](#pesquisar-certificados)
      * [Acessar Certificado](#acessar-certificado)
      * [Validar Certificado](#validar-certificado)
    * [Recursos Privados](#recursos-privados) 
      * [Gerar Certificado](#gerar-certificado)
      * [Deletar Certificado](#deletar-certificado)
      * [Adicionar Modelo](#adicionar-modelo)
      * [Remover Modelo](#remover-modelo)
  * [Notificação](#notificação)
    * [Adicionar Membro](#adicionar-membro) 
    * [Remover Membro](#remover-membro) 
    * [Adicionar Projeto](#adicionar-projeto) 
    * [Remover Projeto](#remover-projeto) 
    * [Agendar Atividade](#agendar-atividade) 
    * [Remover Atividade](#remover-atividade) 
    * [Editar Atividade](#editar-atividade) 
    * [Listar Atividades](#listar-atividades) 
  * [Atas](#atas)
    * [Adicioar Pautas](#adicionar-pautas)
    * [Editar Pautas](#editar-pautas)
    * [Criar Ata](#criar-ata)
    * [Submeter Ata](#submeter-ata)

Formato dos Dados
=================
O único formato de dados suportados atualmente é o JSON.

Erros padrões da API
====================
Erros são indicados pelos erros padrões do HTTP. Informações adicionais
podem ser incluídas no JSON retornado em uma requisição.

### Lista de Erros/Descrição:

|Código|Descrição                                         |
|------|--------------------------------------------------|
|400   |os parametros passados não estão de acordo. Idealmente o erro deve acompanhar uma mensagem indicando o porque.|
|403   |a requisição está tentando acessar um recurso que possui autentição.|
|404   |recurso não existe ou não encontrado.|
-----------------------------------------------------------


Certificado
===========
Esse serviço é responsável por gerenciar a parte da certificação dos eventos oferecidos pelo grupo PETComp.

Recursos Públicos
-----------------
Os recursos públicos podem ser acessados por qualquer usuário do sistema, não precisando de nenhum tipo de credencial ou nível de acesso.
		
Pesquisar Certificados
----------------------
**Descrição**: 
O usuário quer procurar algum certificado de um evento que participou. Uma forma de fazer isso é listar os certificados a partir de algum critério de busca que pode ser pelo evento (i.e Palestra, minicurso, etc), pelo título do evento, ano ou pelo seu próprio nome ou identificador.<br>
**URL**: `/certificados/pesquisas`<br>
**Método**: GET<br>
**Parâmetros**: <br>
  `criterio` indicar o critério utilizado para buscar os certificados a serem listados.<br>
  `valor` indicar o argumento utilizado na busca i.e o nome do usuário.<br>
**Retorno**: 
uma lista com as informações de cada certificado compatível com os critérios da busca. Observação: a duração do evento é em minutos; caso o evento ocorra em mais de um dia o formato é "DD de MM até DD de MM de AA"
					 
Exemplo de resposta:

```json
 {
  "certificados": [
    {
      "nome": "Manoel Moura",
      "evento": "CiPET",
      "titulo": "Como descascar uma fruta",
      "duracao": 120,
      "data": "16 de fevereiro de 2017",
      "palestrante": "Moura Pai"
    }
  ]
 }
```

Acessar Certificado
-------------------
**Descrição**: O usuário deseja acessar um certificado.<br>
**URL**: `/certificados/{cid}`<br>
**Método**: GET<br>
**Parâmetros**: <br>
`cid` o identificador do certificado<br>
**Retorno**:<br>
o certificado é enviado através de uma representação em string binária. É responsabilidade da aplicação decidir como mostrá-lo para o usuáro.<br>

Exemplo de resposta:

```json
{
  "dados": "6b6a6471776b6a646271776a62646b61776..."
}
```
Validar Certificado
-------------------
**Descrição**: O usuário deseja verificar se um determinado certificado é válido e não uma falsificação.<br>
**URL**: `/certificado/validos/{cid}`<br>
**Método**: GET<br>
**Parâmetro**:<br>
`cid` o identificador do certificado.<br>
**Retorno**:<br>
caso ele exista envia um conjunto de informações para conferir com os dados presentes no certificados, caso contrário retorna um JSON vazio.<br>

Exemplo de resposta:

```json
{
  "dados": {
  "nome": "Manoel Moura",
  "titulo": "Como descascar uma batata",
  "duracao": 120,
  "data": "16 de fevereiro de 2017"
  }
}
```

Recursos Privados
-----------------
Recursos privados necessitam de credenciais para ser acessados, pois envolve modificar a base de dados do serviço.

Gerar Certificado
-----------------
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/certificados/gerador`<br>
**Método**: POST<br>
**Parâmetros**:<br>
`ministrante` quem ministrou o evento.<br>
`evento` tipo do evento.<br>
`titulo` título do evento.<br>
`duracao` quanto tempo o evento durou em minutos.<br
`data` data que o evento foi realizado.<br>
`participantes` uma lista contendo o nome e email das pessoas que participaram do evento.<br>
**Retorno**:<br>
uma lista com as informações dos certificados gerados.

Exemplo de resposta:
```json
 {
  "certificados": [
    {
      "nome": "Manoel Moura",
      "evento": "CiPET",
      "titulo": "Como descascar uma fruta",
      "duracao": 120,
      "data": "16 de fevereiro de 2017",
      "palestrante": "Moura Pai"
    }
  ]
 }
```

Deletar Certificado
-------------------
**Descrição**: remove um ou mais certificados da base de dados.<br>
**URL**: `/certificados/gerador`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
`titulo` título do evento presente no certificado.<br>
`participantes` lista de contendo o nome participantes que deseja remover os certificados.<br>
**Retorno**:<br>
uma lista com as informações dos certificados restantes do evento.

Exemplo de resposta:
```json
 {
  "certificados": []
 }
```

Adicionar Modelo
------------------
**Descrição**: adiciona um modelo usado como base para gerar os certificados.<br>
**URL**: `/certificados/modelos`<br>
**Método**: POST<br>
**Parâmetros**:<br>
`nome` um nome para identificar o modelo.<br>
`evento` o tipo de evento que ele está associado.<br>
`modelo` uma string binária contendo o modelo.<br>
`formato` o formato de arquivo do modelo.<br>
**Retorno**:<br>
um arquivo JSON vazio.

Remover Modelo
----------------
**Descrição**: remove um modelo presente na base de dados.<br>
**URL**: `/certificados/modelos`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
`nome` o nome que identifica o modelo.<br>
`evento` o tipo de evento que ele está associado.<br>
**Retorno**:<br>
um arquivo JSON vazio.

Notificação
===========
Notifica aos membros do grupo PETComp as atividades pendendes a serem realizadas.

Adicionar Membro
----------------
**Descrição**: adiciona um novo membro na base de dados com informações necessárias para poder notificá-lo caso necessário. Se invocado com o mesmo `ra` sobrescreve os dados na base de dados.<br>
**URL**: `/notificador/membros`<br>
**Método**: POST<br>
**Parâmetros**:<br>
`ra` registro acadêmico do novo membro para identificá-lo de forma única.<br>
`nome` nome do novo membro.<br>
`email` email do novo membro.<br>
**Retorno**:<br>
o identificador gerado para aquele membro.

Remover Membro 
--------------
**Descrição**: remove um membro da base de dados caso ele não faça mais parte do grupo.<br>
**URL**: `/notificador/membros`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
`ra` registro acadêmico do ex-membro.<br>
**Retorno**:<br>
um JSON vazio.

Adicionar Projeto
-----------------
**Descrição**: adiciona um projeto a base de dados, que poderá ser usado como forma de notificar um grupo de membros.<br>
**URL**: `/notificador/projetos`<br>
**Método**: POST<br>
**Parâmetros**:<br>
`nome` nome do novo projeto.<br>
**Retorno**:<br>
o identificador gerado para aquele projeto.

Remover Projeto 
---------------
**Descrição**: remove um projeto descontinuado ou finalizado da base de dados.<br>
**URL**: `/notificador/projetos`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
`nome` nome do projeto que será removido.<br>
**Retorno**:<br>
um JSON vazio.

Agendar Atividade
-----------------
**Descrição**: agenda uma atividade que será notificada aos responsáveis por ela.<br>
**URL**: `/notificador/atividades`<br>
**Método**: POST<br>
**Parâmetros**:<br>
`nome` nome da atividade.<br>
`descricao` uma descrição sobre a atividade.<br>
`projeto` o projeto que a atividade faz parte<br>
`data-inicio` quando a atividade começará.<br>
`data-fim` quando a atividade encerrará.<br>
`metodo` uma lista contendo os métodos de notificação que serão usados.<br>
`prioridade` um nível de prioridade para que a atividade seja realizado dentro do prazo.<br>
`responsaveis` uma lista contendo os membros responsáveis pela atividade. Caso o nome do projeto seja passado, todos os responsáveis por aquele projeto serão notificados.<br>
**Retorno**:<br>
um identificador gerado para aquela atividade.

Remover Atividade
-----------------
**Descrição**: cancela uma atividade agendada para ser notificada.<br>
**URL**: `/notificador/atividades`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
`nome` registro acadêmico do novo membro para identificá-lo de forma única.<br>
**Retorno**:<br>
um JSON vazio.

Editar Atividade
----------------
**Descrição**: Altera alguma informação de uma atividade criada.<br>
**URL**: `/notificador/atividades`<br>
**Método**: PUT<br>
**Parâmetros**:<br>
Os mesmos utilizados para criar uma atividade, porém todos são opcionais.
`id` o identificador da atividade.<br>
`nome` nome da atividade.<br>
`projeto` o projeto que a atividade faz parte.<br>
`descricao` uma descrição sobre a atividade.<br>
`data-inicio` quando a atividade começará.<br>
`data-fim` quando a atividade encerrará.<br>
`metodo` uma lista contendo os métodos de notificação que serão usados.<br>
`prioridade` um nível de prioridade para que a atividade seja realizado dentro do prazo.<br>
`responsaveis` uma lista contendo os membros responsáveis pela atividade. Caso o nome do projeto seja passado, todos os responsáveis por aquele projeto serão notificados.<br>
**Retorno**:<br>
um JSON contendo os valores dos dados antigos comparando com os novos.

Exemplo de resposta:
```json
 {
  "nome-antigo": "Reunião Urgente",
  "nome-novo": "Reunião Não Mais Urgente",
  "prioridade-antiga": "alta",
  "prioridade-nova": "baixa"
 }
```

Listar Atividades
-----------------
**Descrição**: listar todas as atividades agendadas que satisfação alguns critérios de filtragem.<br>
**URL**: `/notificador/listar`<br>
**Método**: GET<br>
**Parâmetros**:<br>
`membro` uma lista com os membros que devem fazer parte das atividades que serão listadas.<br>
`projeto` uma lista com os projetos que devem fazer parte das atividades que serão listadas.<br>
`prioridade` uma lista de prioridade válidas das atividades que serão listadas.<br>
**Retorno**:<br>
uma lista com as informaçõs das atividades encontradas.

Exemplo de reposta:
```json
 {
  "id": "8d189j21",
  "nome": "Reunião Não Mais Urgente",
  "descricao": "reunião para discutir sobre a compra de bolachas",
  "data-inicio": "Fri, 31 Jan 2042 21:01:05 +0000",
  "data-fim": "Fri, 31 Jan 2042 22:01:05 +0000",
  "metodo": [
   "email"
  ],
  "prioridade": "baixa",
  "responsaveis": [
   "Joaquim Q.", "Tomas", "Memória"
  ]
 }
```

Atas
====
Gerar as atas das reuniões do PETComp.

Adicionar Pautas
----------------
**Descrição**: adicionar algum item de pauta a ser discutido na reunião.<br>
**URL**: `/atas/pautas`<br>
**Método**: POST<br>
**Parâmetros**:<br>
`item` uma lista contendo o título dos items de pauta.<br>
**Retorno**:<br>
os items de pauta atuais.

Exemplo de resposta:
Exemplo de reposta:
```json
 {
  "items": ["Feedback CinePET", "Feedback CiPET", "Feedback ConfraPET"]
 }
```

Remover Pautas
-------------
**Descrição**: remover algum item de presente na pauta.<br>
**URL**: `/atas/pautas`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
`item` uma lista contendo o título dos items de pauta.<br>
**Retorno**:<br>
os items de pauta restantes.

Exemplo de resposta:
Exemplo de reposta:
```json
 {
  "items": []
 }
```

Criar Ata
-------------------
**Descrição**: acessa um modelo que será usado para escrever a ata.<br>
**URL**: `/atas/modelos`<br>
**Método**: GET<br>
**Parâmetros**: Nenhum<br>
**Retorno**:<br>
A ser decidido.

Submeter Ata
----------------
**Descrição**: submete uma ata após redigida para correção, que poderá ser utilizada para extrair informações para gerar agendar atividades automaticamente.<br>
**URL**: `/atas/submissoes`<br>
**Método**: POST<br>
**Parâmetros**:<br>
A ser decidido.
**Retorno**:<br>
A ser decidido.

