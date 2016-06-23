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

Feito isso, vamos criar um Schema chamado 'UserSchema' para servir como modelo base para a collection 'User':

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

const User = Mongoose.model('User', PessoaSchema);
```
Criamos também o nosso model, bom agora vamos criar quantos usuario no banco de dados forem necessários:

```javascript
let userNew = new User();

userNew.nome = 'Pedro';
userNew.idade = 23;
userNew.save((err, user) => {
    console.log(user);
});


let userNew = new User();

userNew.nome = 'Joao';
userNew.idade = 37;
userNew.endereco = {rua: '84C', numero:'93', bairro:'Centro' };
userNew.save((err, user) => {
    console.log(user);
});
```

Bom, agora nós vamos apresentar os usuarios já cadastrados

```javascript

User.find(function(err, users){
    users.forEach(usuario => {
        console.log(usuario.nome);
    });
});
```

Certo, mas e o que acontece se você tentar acessar o endereco? Mostrará undefined, certo? E pior se você tentar acessa a rua o programa lançará uma excessão, então, como resolver isso?

```javascript

User.find((err, users) =>{
	users.forEach(( { nome, idade, endereco = { rua : '', numero : '', bairro : '' } } ) =>{
		console.log(endereco.rua);
	});
});

```
