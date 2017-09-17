# REST API
Alguns serviços oferecidos pelo sistema {nome_do_sistema_geral} são implementados através de microserviços. Eles são escondidos dos usuários, sendo acessado através do {nome_do_sistema_ruby} que funciona como um middleware/proxy? para eles. Essa referência documenta os ponto de acessos desses serviços.
  * `Certificado`
  * `Notificação`
  * `Atas`

## Formato dos dados
O único formato de dados suportados atualmente é o JSON.

## Erros padrões da API
Erros são indicados pelos erros padrões do HTTP. Informações adicionais
podem ser incluídas no JSON retornado em uma requisição.

### Lista de erros/descrição:

|Código|Descrição                                         |
|------|--------------------------------------------------|
|400   |os parametros passados não estão de acordo. Idealmente o erro deve acompanhar uma mensagem indicando o porque.|
|403   |a requisição está tentando acessar um recurso que possui autentição.|
|404   |recurso não existe ou não encontrado.|
-----------------------------------------------------------


## Certificado
Esse serviço é responsável por gerenciar a parte da certificação dos eventos oferecidos pelo grupo PETComp.

### Recursos Publicos
  Os recursos públicos podem ser acessados por qualquer usuário do
  sistema, não precisando de nenhum tipo de credencial o nível de
  acesso.
		
#### Pesquisar Certificados
**Descrição**: 
O usuário quer procurar algum certificado de um evento que participou. Uma forma de fazer isso é listar os certificados a partir de algum critério de busca que pode ser pelo evento (i.e Palestra, minicurso, etc), pelo título do evento, ano ou pelo seu próprio nome ou identificador.<br>
**URL**: `/certificados/pesquisa`<br>
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

#### Acessar Certificado
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
#### Validar Certificado
**Descrição**: O usuário deseja verificar se um determinado certificado é válido e não uma falsificação.<br>
**URL**: `/certificado/valido/{cid}`<br>
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

### Recursos Privados
Recursos privados necessitam de credenciais para ser acessados, pois envolve modificar a base de dados do serviço.
		
#### Gerar Certificado
**Descrição**: após um evento é necessário gerar os certificados para serem armazenados na base de dados.<br>
**URL**: `/certificados/gerador`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

#### Deletar certificado
**Descrição**: <br>
**URL**: `/certificados/`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

#### Adicionar template
**Descrição**: <br>
**URL**: `/certificados/`<br>
**Método**: POST<br>
**Parâmetros**:<br>
**Retorno**:<br>

#### Remover template
**Descrição**: <br>
**URL**: `/certificados/`<br>
**Método**: DELETE<br>
**Parâmetros**:<br>
**Retorno**:<br>

## Notificação


## Atas


