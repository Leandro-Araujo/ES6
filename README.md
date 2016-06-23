# ES6

## Destructuring and Mongoose

Olá pessoal, vamos começar com a configuração do mongoose.js (Antes de mais nada coloque o 'mongoose em suas dependências):

```javascript
'use strict'
const Mongoose = require('mongoose');

const db = Mongoose.connection;

db.on('error', console.error);
db.once('open', function() {

});

Mongoose.connect('mongodb://localhost/testefine');
```

Feito isso, vamos criar um Schema chamado 'UserSchema' para servir como modelo base para a collectiona 'User':

```javascript
const EnderecoPessoaSchema = new Mongoose.Schema({
	rua: String,
    numero: String,
    bairro: String
});

const PessoaSchema = new Mongoose.Schema({
	nome: String,
	idade: Number,
    endereco: EnderecoPessoaSchema
});

const User = Mongoose.model('Site', PessoaSchema);
```
Criamos também o nosso model, bom agora vamos criar quantos usuario no banco de dados forem necessários:

```javascript
let userNew = new User();

userNew.nome = 'Pedro';
userNew.idade = 23;
userNew.endereco = {rua: '34C', numero:'273', bairro:'Centro' };
userNew.save(function(err, user){
    console.log(user);
});


let userNew = new User();

userNew.nome = 'Joao';
userNew.idade = 37;
userNew.endereco = {rua: '84C', numero:'93', bairro:'Centro' };
userNew.save(function(err, user){
    console.log(user);
});
```
