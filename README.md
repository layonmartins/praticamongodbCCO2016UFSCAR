#Prática mongodb Layon Martins Fonseca
##Mestrado Ufscar, Aula de Banco de Dados
###Apresentação de trabalho NoSql 22/06/2016
####Banco de Dados Orientado a documentos, MongoDB

Estou usando o ubuntu com uma box vagrant, ubuntu 14.04 64X com o mongodb(3.2.7, mas roda no 2.0.4) instalado, configurado,  e executando.
			OBS: Caso precisar! version 3.2.7!
			//#Para iniciar o serviço mongo use: 
>  * sudo service mongod start<br/>
>  * sudo service mongod stop<br/>

Para seguir certifique-se que os processos descritos acima já estejam em funcionamento na sua máquina.

___________________________________________________________

#Comandos básicos vagrant:
 - Ligar máquina -> vagrant up
 - Acessar ssh -> vagrant ssh
 - Deslogar ssh -> Ctrl+D
 - Desligar máquiva virtual -> vagrant halt


#Informações:
- O '#' representa um novo tópico ou assunto.
- As // representa um comentário.
- O * mostra a sintaxe de um comando.
- Está -> (setinha) indica que você deve digitar o comando a frente no Shell Editor mongoDB e precionar enter.
- R: representa um retorno que deve ser obtido.
- &&&&&&&&&&&&&&&&&& preste atenção.
- Obs, é uma explicação.

O tutorial geralmente segue a seguinte regra:
 - Fala sobre uma operação.
 - Explica a sintaxe.
 - E oferece um exemplo para testar.
 - Para entender e preciso consultar o modelo dos dados mostrado nas apresentações do slide.


###########################################################
###################### M O N G O D B ######################
###########################################################

___________________________________________________________
##Acessar mongodb
-> mongo<br/>
R: 	MongoDB shell version: 3.2.7
	connecting to: test
	> 


##Exibir databases
- * show dbs

##Criar um exemplo de teste
- -> use test
- -> db.teste.insert({"HELLO":"WORLD"})
- -> db.teste.find()
- -> db.dropDatabase()

##Criar uma database ou usar uma database
//obs, a db não é criada instantaneamente, e preciso inserir uma collection
- * use nomedatabase
- -> use locadora <br/>
R: switched to db locadora


##Exibir collections
- * show collections


##Verificar uma collection
- * db.nomeCollection.count() //retorna quantidade de documentos
- -> db.ator.count()
- R: 0

___________________________________________________________

##Para criar uma collection, e preciso inserir um documento
- * db.nomeCollection.insert({JSON})


###########################################################
####### Vamos começar criando nossas collections :) #######
###########################################################

##Collection ATOR: obs: o id é nome artistico único na collection

> -> db.ator.insert({
	"_id":"Beatrix",
	"nomeReal":"Uma Thurman",
	"estrela":["Kill Bill"]
})


> -> db.ator.insert({
	"_id":"Will Smith",
	"nomeReal":"Willard Carroll Smith",
	"estrela":[
				"Eu sou a lenda",
				"Hancok",
				"MIB - Homens de Preto"
				]
})


> -> db.ator.insert({
	"_id":"Bruce Lee",
	"nomeReal":"Lee Jun Fan",
	"estrela":[
				"Operação Dragão",
				"A Fúria do Dragão",
				"O voo do dragão",
				"Dragão Chinês"]
})


___________________________________________________________

##Collection FILME: o id é o nome do filme + ano de lançamento
###&&&&&Obs: inserir tudo de uma vez, porem não funciona na versão 2.0.4

> -> db.filme.insert([
	{
		"_id":"killbill2003",
		"nome":"Kill Bill - Volume 1",
		"fitas":["1","2"],
		"estrela":["Beatrix", "Bill"],
		"categoria":"Ação"
	},
	{
		"_id":"killbill2004",
		"nome":"Kill Bill - Volume 2",
		"fitas":["3","4"],
		"estrela":["Beatrix", "Bill"],
		"categoria":"Ação"
	},
	{
		"_id":"eusoualenda2007",
		"nome":"Eu sou a Lenda",
		"fitas":["5"],
		"estrela":["Will Smith"],
		"categoria":"Aventura"
	},
	{
	"_id":"hancock2008",
	"nome":"Hancock",
	"fitas":["6", "7", "8"],
	"estrela":["Will Smith"],
	"categoria":"Aventura"
	},
	{
	"_id":"operacaodragao1973",
	"nome":"Operação Dragão",
	"fitas":["9", "10"],
	"estrela":["Bruce Lee"],
	"categoria":"Ação"
	},
	{
	"_id":"ovoododragao1972",
	"nome":"O Voo Do Dragão",
	"fitas":["11"],
	"estrela":["Bruce Lee"],
	"categoria":"Ação"
	}
])


##&&&&&Inserir os FILMES separadamente p/ versão 2.0.4

> -> db.filme.insert(
	{
		"_id":"killbill2003",
		"nome":"Kill Bill - Volume 1",
		"fitas":["1","2"],
		"estrela":["Beatrix", "Bill"],
		"categoria":"Ação"
	})

> -> db.filme.insert(	
	{
		"_id":"killbill2004",
		"nome":"Kill Bill - Volume 2",
		"fitas":["3","4"],
		"estrela":["Beatrix", "Bill"],
		"categoria":"Ação"
	})

> -> db.filme.insert(
	{
		"_id":"eusoualenda2007",
		"nome":"Eu sou a Lenda",
		"fitas":["5"],
		"estrela":["Will Smith"],
		"categoria":"Aventura"
	})

> -> db.filme.insert(
	{
	"_id":"hancock2008",
	"nome":"Hancock",
	"fitas":["6", "7", "8"],
	"estrela":["Will Smith"],
	"categoria":"Aventura"
	})

> -> db.filme.insert(
	{
	"_id":"operacaodragao1973",
	"nome":"Operação Dragão",
	"fitas":["9", "10"],
	"estrela":["Bruce Lee"],
	"categoria":"Ação"
	})

> -> db.filme.insert(
	{
	"_id":"ovoododragao1972",
	"nome":"O Voo Do Dragão",
	"fitas":["11"],
	"estrela":["Bruce Lee"],
	"categoria":"Ação"
	})


___________________________________________________________

##Collection CLIENTE, obs: se o campo aluga estiver null então o cliente não tem filmes alugados, caso contrário a seguinte estrutura define um aluguel de uma fita:

---------------------------------------
> aluga:[{"filme_id":"id do filme", "nome":"nome do filme", "fita":"id da fita", "data":"data da locação"}]
---------------------------------------

___________________________________________________________

##Obs: inserindo clientes, alguns já com locação de filmes

> -> db.cliente.insert({
	"_id":"1",
	"nome":"João da Silva",
	"aluga":[
				{
					"filme_id":"killbill2003",
					"nome":"Kill Bill - Volume 1",
					"fita":"1",
					"data":"19/06/2016"
				},
				{
					"filme_id":"killbill2004",
					"nome":"Kill Bill - Volume 2",
					"fita":"3",
					"data":"19/06/2016"
				}
			]
	})


> -> db.cliente.insert({
	"_id":"2",
	"nome":"Maria BD",
	"aluga":[
				{
					"filme_id":"hancock2008",
					"nome":"Hancock",
					"fita":"6",
					"data":"20/06/2016"
				}
			]
})


> -> db.cliente.insert({
	"_id":"3",
	"nome":"Gabriel Gallo",
	"aluga":null
})


> -> db.cliente.insert({
	"_id":"4",
	"nome":"Gabriel Alves",
	"aluga":null
})


> -> db.cliente.insert({
	"_id":"5",
	"nome":"Viviana Romero",
	"aluga":null
})




___________________________________________________________

###########################################################
################  Buscar   Dados (Querys)  ################
###########################################################

___________________________________________________________

##Busca todos os documentos
- * db.nomeCollection.find()


##Busca com parâmetros <Condição de Busca>
- * db.nomeCollection.find({parâmetros})

##Deixar a consulta mais legível .pretty()
- * db.nomeCollection.find().pretty()

> -> db.ator.find().pretty()<br/>
> -> db.filme.find().pretty()<br/>
> -> db.cliente.find().pretty()

##Sort, Nosso orderby decrescecente -1, crescente 1
> -> db.filme.find().sort({_id: -1}).pretty()<br/>
> -> db.ator.find().sort({"_id": 1}).pretty()


##Buscar filmes de Aventura {PARÂMETROS}
> -> db.filme.find({"categoria":"Aventura"}).pretty()

##Buscar filme de ação, obs: exibir só o nome
> -> db.filme.find({"categoria":"Ação"},{"nome":true}).pretty()

##Buscar filme de ação, obs: exibir só o nome, sem exibir _id
> -> db.filme.find({"categoria":"Ação"},{"_id":false, "nome":true})

##Buscar somente nome dos clientes, obs: ordenado
> -> db.cliente.find({}, {"_id":false,"nome":true}).sort({"nome":1})

##Buscar nome dos clientes que NÃO estão com filme locados
> -> db.cliente.find({"aluga":null},{"_id":false, "nome":true}).sort({"nome":1})

##Buscar nome dos clientes com filmes pendentes
> -> db.cliente.find({"aluga":{$ne:null}},{"_id":false, "nome":true}).sort({"nome":1})

##Busca o primeiro cliente na collection
> -> db.cliente.findOne()

##Buscar cliente que alugou dois filmes específicos com AND
> -> db.cliente.find({"aluga.filme_id":"killbill2003", "aluga.filme_id":"killbill2004"}).pretty()

##Buscar cliente que alugou um filme ou outro com OR
> -> db.cliente.find({$or:[{"aluga.filme_id":"eusoualenda2007"}, {"aluga.filme_id":"hancock2008"}]}).pretty()

###Operadores de busca:
-	$gt maior que (greater-than)
-	$gte igual ou maior que (greater-than or equal to)
-	$lt menor que (less-than)
-	$lte igual ou menor que (less-than or equal to)
-	$ne não igual (not equal)
-	$in existe em uma lista
-	$not traz o oposto da condição
-	$mod calcula o módulo
-	$exists verifica se o campo existe
-	entre outros


___________________________________________________________

###########################################################
################  Update ATENÇÃO - cuidado ################
###########################################################

___________________________________________________________

##Update
Obs: Tome cuidado com o update além de alterar um valor, 
e possível alterar toda a estrutura do documento e removendo campos sem querer, você pode acabar com sua vida!!!

###Por exemplo:

- * db.nomeCollection.update(
	{"_id":"buscando um id"},
	{"nomeCampoparaAlterar":"novoValor"})

> obs: ao executar este comando, o documento correspondente a esse _id teve o nomeCamp. alterado, porém todos os outros campos foram apagados, caso o documento tivesse.


##Para ter um comportamento igual do update SQL, usamos o set
- * db.nomeCollection.update(
	{"creterioDeBusca1":"valor1",...},
	{ $set: {"campoParaAtualizar1":"novoValor1"},...}
)

> Obs: Agora sim ele só vai alterar o campo passado no set, os outros que estiver no documente irão permanecer sem alteração.



___________________________________________________________

##Alugar um filme
obs: p/ alugar o filme, na collection cliente acrescentamos com um update os dados de um aluguel de um filme

> -> db.cliente.update(
	{"_id":"3"},
	{ $set: {"aluga": [
					{
						"filme_id" : "operacaodragao1973",
						"nome" : "Operação Dragão",
						"fita" : "9",
						"data" : "21/06/2016"
					},
					{
						"filme_id" : "ovoododragao1972",
						"nome" : "O Voo Do Dragão",
						"fita" : "11",
						"data" : "21/06/2016"
					}
					]}}
	)

<br/>
> -> db.cliente.find().pretty()


___________________________________________________________

##Devolver um filme
> obs: p/ devolver, na collection cliente alteramos o campo aluga para o valor null, informando que não há mais filme alugados

> -> db.cliente.update(
		{"_id":"1"},
		{ $set: {"aluga":null}}
	)


___________________________________________________________

##Devolver TODOS de uma vez:
Obs: Por padrão o update só altera o primeiro registro que obedece a condição de busca, para alterar toda a collection e preciso ativar a propriedade 'multi' ou usar o .updateMany()

> &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&<br/>
> Only New version 3.2 ++, use: updateMany()

> -> db.cliente.update(
		{ },
		{ $set: {"aluga":"hey"}},
		false, true
	)
<br/>
//this:
> -> db.cliente.updateMany(
		{ },
		{ $set: {"aluga":null}}
	)


___________________________________________________________

###########################################################
################      Remover  dados       ################
###########################################################

___________________________________________________________

##Remover
//Para excluir um documento use:
- * db.nomeCollection.remove({"campBusca":"valorBusca"}) 

> -> db.filme.remove({"_id":"ovoododragao1972"})<br/>
> -> db.filme.find({},{"_id":1})

##Para remover todos os registros de uma collection:
- * db.nomeCollection.remove({})<br/>
-> db.ator.remove({})<br/>
-> db.ator.count()

##Para remover a Collection
- * db.nomeCollection.drop()<br/>
-> db.cliente.drop()

##Apagar a database
Obs: apaga a database que está em use
> -> db.dropDatabase()<br/>
> -> show dbs



___________________________________________________________

###########################################################
#################        THE   END         ################
###########################################################

___________________________________________________________


###Espero que estes exemplos foram uteis, e fáceis de se compreender, obrigado por acompanhar.

##God bless you.

