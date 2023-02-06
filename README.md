<h1 align="center"> APOLLO </h1>
Projeto final do curso Programadores Cariocas, realizado em dupla, com o objetivo de aplicar o que foi aprendido no módulo 5 em um site. Este projeto, intitulado "APOLLO", foi criado com base em uma ideia da dupla Adriano Rodrigues e João Faustino, enfocada na programação e tecnologia. Assim, decidiram criar um site de vendas de eletrônicos, com ênfase em smartphones.

## Tecnologias utilizadas para elaboração e organização do projeto ⤵️

- Keep (do Google)
- Drive (do Google)
- Canva
- GitHub
- VS Code
- Workbench / MySql
- Node.js
- Xampp
- Whatsapp


## O passo a passo do projeto foi da seguinte forma ⤵️

- Instale o VS Code, siga as configurações recomendadas e abra.
- Instale o Node.js, siga as configurações recomendadas e salve.
- Abra o prompt de comando e digite e aperte enter.:

```
npm install -g npm
```

- Para verificar se está tudo certo, dê os comandos:


```
node -v
```


```
npm -v
```

Parte para idealização do projeto:

- Criamos uma pasta na área de trabalho
- Abrimos o VS Code
- Criamos um arquivo index.js na pasta criada anteriormente
- Abrimos o terminal para dar os seguintes comandos:

```
npm init -y
```
```
npm install express
```
```
npm install nodemon
```
```
npm install express-handlebars
```

Agora configurações para fazer a aplicação rodar sem erros:

- No arquivo package.jason adicionamos o seguinte código abaixo de "test":

```
"start": "nodemon ./index localhost 3000"
```

- Adicionamos uma virgula no fim do TEST

Já no index.js, adicionamos:

```
const express = require('express') const {engine} = require('express-handlebars')

const app = express()

app.engine('handlebars', engine()) app.set('view engine', 'handlebars')

app.use(express.static("public"))

app.get('/', (req, res) =>{

res.render('home')
})

app.listen(3000, () =>{ console.log('App rodando...')})
```

Já no main.handlebars adicionamos:

```
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Exercicio</title>
    <link rel="stylesheet" href="/css/styles.css">
</head>
<body>
    {{{body}}}
</body>
</html>
```

- Por fim, adicionamos uma h1 em home.handlebars para testar no servidor.

- Demos o comando: 

```
npm start
```

- E abrimos o http://localhost:3000/

- Toda construção foi baseado na arquitetura MVC

## Banco de dados, sua documentação e implementação do CRUD ⤵️

- Primeiro, baixamos o XAMPP e o WorkBench MYSQL.
- Iniciamos os programas com as configurações recomendadas deles.
- Abrimos o XAMPP primeiro e apertamos em "start" ao lado do nome "MySql".
- Abrimos o Workbench e criamos um terminal no "+".
- Ao entrar ele estava em "Administration", mas mudamos para "Schemas".

Então entramos na parte da criação do banco de dados:

Demos os seguintes comandos:

```
create database apollo;
```

```
use apollo;
```

Configuramos o que seria exibido da seguinte forma:

```
Create table smartphone(
ID INT(5) NOT NULL AUTO_INCREMENT PRIMARY KEY,
smartphone VARCHAR(100) NOT NULL,
lançamento VARCHAR (100) NOT NULL,
versaoAtual VARCHAR (100) NOT NULL,
ProxVersao VARCHAR (100) NOT NULL,
Preço FLOAT(8) NOT NULL );
```

Por fim, demos um:

```
select * from smartphone
```

-Isso foi feito para visualizar a tabela.

Agora a parte mais importante, como adicionar e integrar essas informações do Workbench em nosso código no VS Code?

- Declaramos o MySQL para poder usá-lo em nosso projeto com:

```
const mysql = require ('mysql')
```

- Esse código fica no topo da página de index.js, abaixo das duas primeiras opções previamente adicionadas.
- Precisavamos realizar a conexão do nosso projeto com o banco de dados, então usamos:

```
const conn = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password:'',
    database: 'apollo'
})
```

- Note que o host definimos como localhost, o user pré definido no início do Workbench, nenhuma senha e database (o nome do banco) como apollo.

Agora de fato nosso CRUD:

- Criamos duas pastas .handlebars em "views" chamadas editar.handlebars e inserir.handlebars.

Em editar definimos uma página para editar, visualizar ou excluir as seguintes informações de um produto:

- Nome do modelo
- Data de lançamento
- Versão atual
- Próxima versão
- Preço

Precisariamos declarar as edições acima, portanto para criar as rotas das informações entre o código no VS Code e no banco de dados, alguns métodos/códigos foram usados no index.js:

```
app.get('/adm/editar/:ID', (req, res) =>{
    const ID = req.params.ID
    const sql = (`SELECT * FROM smartphone where id = ${ID}`)

    
    conn.query(sql, function (err, data){
        if (err) {
            console.log(err)
            return
        }

        const smartphone = data[0]

        res.render('./crud/editar', {smartphone})
    })
})
```

- O código acima foi declarado para visualizar informações

```
app.post('/adm/remove/:ID', (req, res) =>{
    const ID = req.params.ID
    const sql = `DELETE FROM usuarios WHERE ID = ${ID}`

    conn.query(sql, function(err, data){
        if (err) {
            console.log(err)
            return
        }
        res.redirect('/adm/inserir')
    })
})
```

- O código acima foi declarado para deletar informações.

```
app.post('/editar/atualizar',(req, res) =>{
    const ID = req.body.ID
    const smartphone = req.body.smartphone
    const lançamento = req.body.lançamento
    const versaoAtual = req.body.versaoAtual
    const ProxVersao = req.body.ProxVersao
    const Preço = req.body.Preço

    const sql = `UPDATE smartphone set smartphone = '${smartphone}', lançamento =  '${lançamento}', versaoAtual = '${versaoAtual}', ProxVersao = '${ProxVersao}', Preço = '${Preço}' where ID = ${ID};`

    conn.query(sql, function(err, data){
        if(err){
            console.log(err)
            return
        }
        res.redirect('/adm/inserir')
    })
})
```

- O código acima foi declarado para atualizar informações.

Por fim:

```
app.post('/apollo/insert', (req, res) =>{
    
    const smartphone = req.body.smartphone
    const lançamento = req.body.lançamento
    const versaoAtual = req.body.versaoAtual
    const ProxVersao = req.body.ProxVersao
    const Preço = req.body.Preço


    const sql = `INSERT INTO smartphone(smartphone , lançamento, versaoAtual, ProxVersao, Preço) values ('${smartphone}', '${lançamento}', '${versaoAtual}' , '${ProxVersao}', '${Preço}');`

    conn.query(sql, function(err){
        if(err){
            console.log(err)
        }
        res.redirect('/adm/inserir')
    })
})
```

- O código acima foi declarado para inserir informações.

## Páginas e uso do CRUD ⤵️

- O crud foi usado especialmente para as páginas "Atualizações" e "Login", construídas da mesma forma citada acima.
- Nela, o usuário pode ver informações do smartphone, como modelo, lançamento, versão atual, próxima versão e preço.
- A página é atualizada automaticamente com a implementação do CRUD.

Mas como isso é feito?

- Criamos uma página de "Administradores"
- É necessário um email válido e senha com números, letras e caracteres especiais para entrar na área administrativa.

Ao entrar na área administrativa:

- Há a visualização (GET) dos dados que já foram ou serão inseridos.
- O método POST foi declarado para atuar aqui, onde é possível inserir informações.

Há uma opção no canto da tela chamada "Editar" e ao clicar somos levador à:

- Página de edição e delete.
- Aqui os métodos POST de editar e deletar foram implementados.
- Todas as opções citadas acima são atualizadas automaticamente.

## As rotas criadas foram:  ⤵️

- Início/Home
- Loja
- Produtos
- Suporte
- Sobre
- Atualizações
- Administrador/adm
- Login
- Cadastro
- ApolloP11lite
- ApolloP11pro
- ApolloP12lite
- ApolloP12pro
- ApolloP13deluxe
- Noticia1
- Noticia2
- Noticia3

## Passo a passo para baixar e rodar o projeto ⤵️

- Clique em code na parte superior do projeto 

- Dowlond zip

- Extraia os arquivos

- Abra o projeto no vsCode

- Abra o terminal e escreva os comandos:


```
npm i ou npm install
```


```
npm start 
```

## Responsáveis ⤵️

- Adriano Rodrigues - páginas handlebars, css, javascript e readme.
- João Faustino - backend, implementação do CRUD e diagrama.
