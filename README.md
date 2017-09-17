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
      "data": "16 de fevereiro de 2017"
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
=================
Recursos privados necessitam de credenciais para ser acessados, pois envolve modificar a base de dados do serviço.

Gerar Certificado
-----------------
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/certificados/gerador`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

Deletar Certificado
-------------------
**Descrição**: <br>
**URL**: `/certificados/gerador`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

Adicionar Modelo
------------------
**Descrição**: <br>
**URL**: `/certificados/modelos`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

Remover Modelo
----------------
**Descrição**: <br>
**URL**: `/certificados/modelos`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

Notificação
===========
Notifica aos membros do grupo PETComp as atividades pendendes a serem realizadas.

Adicionar Membro
----------------
**Descrição**:<br>
**URL**: `/notificador/membros`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

Remover Membro 
--------------
**Descrição**:<br>
**URL**: `/notificador/membros`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

Adicionar Projeto
-----------------
**Descrição**:<br>
**URL**: `/notificador/projetos`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

Remover Projeto 
---------------
**Descrição**:<br>
**URL**: `/notificador/projetos`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

Agendar Atividade
-----------------
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/notificador/atividades`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

Remover Atividade
-----------------
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/notificador/atividades`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

Editar Atividade
----------------
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/notificador/atividades`<br>
**Método**: PUT<br>
**Parâmetros**:<br>
**Retorno**:<br>

Listar Atividades
-----------------
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/notificador/{por}/listar`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

## Atas
Gerar as atas das reuniões do PETComp

Adicionar Pautas
----------------
**Descrição**:<br>
**URL**: `/atas/pautas`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

Editar Pautas
-------------
**Descrição**:<br>
**URL**: `/atas/pautas`<br>
**Método**: PUT<br>
**Parâmetros**:<br>
**Retorno**:<br>

Criar Ata
-------------------
**Descrição**:<br>
**URL**: `/atas/modelos`<br>
**Método**: GET<br>
**Parâmetros**:<br>
**Retorno**:<br>

Submeter Ata
----------------
**Descrição**:<br>
**URL**: `/atas/submissoes`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>


