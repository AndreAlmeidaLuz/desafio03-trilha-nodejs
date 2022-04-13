# desafio03-trilha-nodejs
# Sobre o desafio

Nesse desafio, você realizará consultas no banco de dados com o TypeORM de três maneiras:

- Usando o ORM
- Usando Query Builder
- Usando Raw Query

Isso irá te ajudar a entender e exercitar os diferentes tipos de consultas que podemos fazer.

No template, você irá encontrar uma aplicação já estruturada (apenas as entidades e repositórios) onde você deverá completar o que falta nas consultas dos dois repositórios.

A aplicação possui dois módulos: `users` e `games`. Um **usuário** pode ter vários jogos e um mesmo **jogo** pode estar associado a vários usuários.

## Preparando o ambiente para os testes

Para que os testes funcionem, é importante que você crie uma **database no banco Postgres** com o nome `queries_challenge` e substitua os dados de autenticação (caso os seus não sejam os mesmos) no arquivo **ormconfig.json**: 

![https://cdn.discordapp.com/attachments/719251629773422612/874276443515945011/unknown.png](https://cdn.discordapp.com/attachments/719251629773422612/874276443515945011/unknown.png)

Se você não possui um container do Docker rodando o Postgres, é possível criá-lo com seguinte comando:

```bash
docker run --name ignite-challenge-database-queries -e POSTGRES_DB=queries_challenge -e POSTGRES_PASSWORD=docker -p 5432:5432 -d postgres
```

Com isso, o container com o banco Postgres será instalado e ficará rodando pronto para você começar implementar o desafio.

Quando o desafio estiver finalizado, você pode parar o container com o comando `docker stop ignite-challenge-database-queries` para que não fique consumindo recursos desnecessários da sua máquina. E caso você tenha parado o container ou reiniciou sua máquina mas ainda precisa do container rodando, é possível iniciá-lo novamente com o comando `docker start ignite-challenge-database-queries`.

## Repositórios da aplicação

Com o repositório criado a partir do template e clonado na sua máquina, navegue até os arquivos **`src/modules/users/repositories/implementations/UsersRepository.ts`** e **`src/modules/games/repositories/implementations/GamesRepository.ts`**. 
Esses deverão ser completados para que os testes sejam satisfeitos.

### UsersRepository

- Método **findUserWithGamesById**
    
    Esse método deve receber o **Id** de um usuário e retornar os dados do usuário encontrado juntamente com os dados de todos os **games** que esse usuário possui.
    
    Exemplo de retorno:
    
    ```jsx
    {
    	id: '81482ac4-29bd-497f-b71a-8ae3b20eca9b',
    	first_name: 'John',
    	last_name: 'Doe',
    	email: 'mail@example.com',
    	created_at: '2021-03-19 19:35:09.877037',
    	updated_at: '2021-03-19 19:35:09.877037',
    	games: [
    		{
    			id: '63a6c35a-ac97-4773-9021-fb93973c8139',
    			title: 'GTA V',
    			created_at: '2021-03-19 19:35:09.877037',
    			updated_at: '2021-03-19 19:35:09.877037',
    		},
    		{
    			id: '74e4fc3b-434d-4452-94eb-27a85dce8d1a',
    			title: 'Among Us',
    			created_at: '2021-03-19 19:35:09.877037',
    			updated_at: '2021-03-19 19:35:09.877037',
    		}
    	]
    }
    ```
    
- Método **findAllUsersOrderedByFirstName**
    
    Esse método deve retornar a listagem de usuários cadastrados em ordem alfabética (**ASC**).
    
    Lembre-se que aqui deve ser usado **raw query** para a consulta.
    
- Método **findUserByFullName**
    
    Esse método deve receber `first_name` e `last_name` e retornar um usuário que possua os mesmos `first_name` e `last_name`. Aqui você deve encontrar o usuário ignorando se o argumento passado está em caixa alta ou não. 
    
    Por exemplo, suponhamos que existe um usuário onde o `first_name` é `Danilo` e o `last_name` é `Vieira`. O método deve retornar o usuário mesmo que os argumentos passados sejam `daNiLo` para `first_name` e `vIeiRA` para `last_name`. Essa consulta deve ser realizada utilizando **raw query** e você pode buscar pelo uso do **LOWER** no Postgres para resolver esse problema.
    

### GamesRepository

- Método **findByTitleContaining**
    
    Esse método deve receber parte do título de um jogo ou o título inteiro e retornar um ou mais jogos que derem match com a consulta. 
    
    Se o método for chamado com o argumento `"or S"` e existir algum jogo com essa sequência de letras no título, o retorno deve ser feito, como por exemplo o seguinte retorno:
    
    ```jsx
    [
    	{
    		id: '63a6c35a-ac97-4773-9021-fb93973c8139',
    		title: 'Need F**or S**peed: Payback',
    		created_at: '2021-03-19 19:35:09.877037',
    		updated_at: '2021-03-19 19:35:09.877037',
    	},
    	{
    		id: '74e4fc3b-434d-4452-94eb-27a85dce8d1a',
    		title: 'Need F**or S**peed: Underground',
    		created_at: '2021-03-19 19:35:09.877037',
    		updated_at: '2021-03-19 19:35:09.877037',
    	}
    ]
    ```
    
    A consulta também deve ser feita de forma case insensitive, ignorando caixa alta onde no banco não existe. Para exemplo, considerando a busca exemplificada acima, o retorno deve ser o mesmo caso o parâmetro passado seja uma string `"nEEd"`. 
    
    Você pode buscar pelo uso do **ILIKE** no Postgres para resolver esse problema. Lembre-se que aqui deve ser usado **query builder** para realizar a consulta.
    
- Método **countAllGames**
    
    Esse método deve retornar uma contagem do total de games existentes no banco. Deve ser usada **raw query** para essa consulta.
    
- Método **findUsersByGameId**
    
    Esse método deve receber o `Id` de um game e retornar uma lista de todos os usuários que possuem o game do `Id` informado. 
    
    Exemplo de retorno:
 ```jsx
[
	{
		id: '81482ac4-29bd-497f-b71a-8ae3b20eca9b',
		first_name: 'John',
		last_name: 'Doe',
		email: 'mail@example.com',
		created_at: '2021-03-19 19:35:09.877037',
		updated_at: '2021-03-19 19:35:09.877037'
	},
	{
		id: '75920ac4-32ed-497f-b71a-8ae3c19eca9b',
		first_name: 'Usuário',
		last_name: 'Qualquer',
		email: 'usuarioqualquer@example.com',
		created_at: '2021-03-19 19:35:09.877037',
		updated_at: '2021-03-19 19:35:09.877037'
	}
]
```
