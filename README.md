[![Build Status](https://travis-ci.org/hugoiuri/mobquestions.svg?branch=master)](https://travis-ci.org/hugoiuri/mobquestions) 


# mobquestions

Repositório de atividade em grupo da disciplina de arquitetura de computação em nuvens da pós graduação de Desenvolvimento de Aplicações Móveis da PUC-MG.

Deseja-se construir uma API Rest para uma aplicação de questões de concursos. Nesta aplicação, 
os usuários podem responder, pesquisar e comentar em questões.

As atividades serão descritas nesta página, e serão progressivamente atualizadas **neste repositório**.

Além das implementações, será necessário realizar o deploy no serviço de plataforma como serviço **heroku**.

## Pré-requisitos

0. Criar uma conta em Github.com (para todos do grupo) e um fork deste repositório  (https://github.com/du2x/mobquestions). No repositório 'forkado', adicionar os demais componentes do grupo como colaboradores.
1. Clonar este repositório. Se necessário, instale o Git for windows disponível em: https://git-scm.com/download/win. Para clonar, rode o comando: 
```sh
git clone https://github.com/<seu_nome_de_usuario>/mobquestions
```
2. Instalar python disponível em https://python.org. Caso esteja utilizando Windows, instalar como Administrador e marcar as duas opções conforme a imagem: https://djangobook.com/wp-content/uploads/figure1_1a.png. 

3. Instale as dependências. Execute no prompt de comando ou no shell: `python -m pip install -r requirements.txt`

4. Defina as variáveis de ambiente *FLASK_APP* e *FLASK_DEBUG*. Em Windows, prompt de comando, execute:
`set FLASK_APP=app.py` e `set FLASK_DEBUG=1` 


5. Para rodar o app flask execute: `python -m flask run --host=0.0.0.0 --port=8088`. Para testar, acesse pelo navegador o endereço http://localhost:8088/

6. Agora realize as modificações em *app.py* para implementar as atividades.

7. Para testar as atividades, recomenda-se a utilização de *POSTMAN* (http://getpostman.com). A versão portável está disponível em https://portapps.github.io/app/postman-portable/. Após instalar, importe `mobquestions.postman_collection.json` para obter os requests das primeiras atividades.





## Atividades

Implemente as seguintes rotas.

0. Done - POST `/v1/users/` (novo usuário)
cadastra um novo usuário, com os dados: username, password, email, name, phones.
retorna status code 201 caso o usuário seja criado; caso o 
*username* enviado já exista na base de dados, retornar status code 409 (Conflict).
exemplo de dados de request: 
```javascript
{"username": "mark", "password": "a123", "email": "mark@knopfler.com", "name": "Mark", "phones": ["3333-2222", "2222-3333"]}
```

1. Done - GET `/v1/users/<username>`  (obtenção de usuário)
retorna os dados do usuário correspondente (pelo username) em formato JSON e o status code 200; ou status code 404 caso o usuário não exista.

2. Done - POST `/v1/authenticate` (autenticação de usuário)
valida a combinação username e password enviadas.
retorna status code 200 em caso de sucesso; e 403, caso a combinação seja inválida, e 400 caso não tenha sido enviados os dois valores: *username* e *password*.
utilize-se a função check_password_hash para comparar o password enviado com o password na base de dados da seguinte forma (por exemplo): `check_password_hash(password_encontrado, password_enviado)`. Esta função retorna True se houver "correspondência".
exemplo de dados de request: 
```javascript
{"username": "mark", "password": "a123"}
```

3. Done - PUT `/v1/users/<username>` (atualização de dados de usuário)
atualiza os dados do usuário correspondente (pelo username). os campos possíveis de modificação são name; email e phones.
```javascript
{"name": "Markin", "phones": ["3333-2222"]}
```

4. Done - PATCH `/v1/users/<username>` (redefinição de senha)
modifica o password do usuário correspondente (pelo username). 
exemplo de dados de request: 
```javascript
{"password": "value"}
```

5. Done - GET `/v1/questions/<question_id>` (obtenção de questão)
retorna os dados da questão correpondente (pelo username) em formato JSON e o status code 200; ou status code 404 caso a questão não exista.


6. Done - POST `/v1/comment` (incluir comentário em questão)
retorna os dados da questão atualizada em formato json e o status code 200 em caso de sucesso.
se a questão não for encontrada, status code 404. se o usuário não for encontrado, ou os dados enviados estiverem inválidos retornar status code 400.
```javascript
{"question_id": "yut4fsd-qs", "username": "mark", "message": "essa questao e facil"}
```

7. Done - GET `/v1/questions/search` (buscar questões)
esta rota receberá como parametro os critérios da busca pela url, como no exemplo abaixo:
/v1/questions/search?disciplina=2&ano=2013
retorna as questões encontradas baseadas nos critérios de busca e o status code 200 em caso de sucesso. retorna status code 400 caso os dados enviados estiverem inválidos.

8. Done - Modifique as implementações das rotas 3. e 6. para que elas requeiram o envio de um token válido. Teste-os com Postman.

9. Done - POST `/v1/questions/answer/`  (responder questao)
retorna se a resposta enviada foi correta ou não. Esta rota deve requerer o envio de um token válido.
Caso a resposta enviada não esteja no domínio de opções válidas, retornar status code 400.
Exemplo de dados enviados:
```javascript
[{"id": "q234541-4c", "answer": "E"},
{"id": "w23as41-5b", "answer": "C"}
]
```


10. Done - GET `/v1/questions/answers` (visualizar respostas)
retorna as respostas enviadas (answer) nas questões respondidas pelo **usuário autenticado**. Observe que para que isso seja possível, a implementação rota 9. deve armazenar as respostas enviadas pelo usuário na coleção de usuários.
Esta rota deve requerer o envio de um token válido. 
O retorno deve ser no formato como do exemplo abaixo:
```javascript
[{"id": "q234541-4c", "answer": "E"},
{"id": "w23as41-5b", "answer": "C"}
]
```

11. Done - POST `/v1/featured_questions` (atualizar perguntas destaque)
atualiza o cache com as perguntas mais respondidas. antes de implementar esta rota, modifique a implementação da rota 9. de modo que inclua um contador de número de respostas na questão sendo respondida. 

12. Done - GET `/v1/featured_questions` (perguntas destaque)
retorna as perguntas mais respondidas. Deve utilizar o cache para obter o resultado.

-----
### Testes

13. Done - Confira se o teste `test_get_user_not_found` executa com sucesso em sua implementação. Caso falhe, modifique em app.py para que este teste execute com sucesso.

14. Done - Implemente o teste  `test_create_user_no_username` que afere que uma requisição para a rota `/v1/create_user` sem o envio de *username* resulta em uma resposta com código de status **400**.

15. Done - Crie e implemente o teste  `test_create_user` que afere que uma requisição para a rota `/v1/create_user` com o envio de *username* e *password* resulta em uma resposta com código de status **201**.

16. Done - Crie e implemente o teste  `test_create_repeated_user` que afere que uma requisição para a rota `/v1/create_user` com o envio de *username* e *password* com *username* já existente resulta em uma resposta com código de status **409**.

17. Done - Crie e implemente o teste `test_answer_question` que afere se o resultado da resposta de uma requisição para `/v1/questions/answer/` retorna o código de status **200** e o resultado conforme o esperado ('C' se a resposta da questão enviada foi certa, 'E' caso contrário). Observe que esta rota demanda o envio de token). Os testes rodam de forma sequencial na ordem em que são escritos. O teste `test_signin` ao ser executado, armazena no atributo `self.token` o token do usuário `fulano`, e estará disponível para todos os testes escritos depois de `test_signin`. Para enviar o token na requisição, será necessário utilizar o parametro `headers` de `self.client` como a seguir, por exemplo: 
```python
self.client.post('/v1/questions/answer',
                headers={'Authorization': 'JWT ' + self.token},
                data=json.dumps({"id": "bc3b3701-b7", "answer": "E"})
                )
```

## Deploy em Heroku

Heroku é um serviço de plataforma como serviço em nuvens.

Para realizar o deploy você deve:

1. Criar uma conta em heroku.com
2. Ao logar em heroku.com você entrará em https://dashboard.heroku.com/apps. Neste painel, acesse o menu "New" no topo e à direita, e escolha a opção "Create new app"
3. Escolha o nome seu_nome_de_usuario_no_github-questions para o seu app e clique em Create app.
4. Agora na seção Deployment method clique em Github e pesquise e escolha o seu repositório mobquestions.
5. Na seção Automatic Deploys, clique em Enable Automatic Deploys. A partir de agora, sempre que você realizar um git push (atualizar o seu repositório), o heroku irá atualizar a aplicação.
6. Na seção Manual deploy. clique em deploy branch. Isso mesmo, o primeiro deploy será manual.
7. Vamos agora adicionar um add-on de cache: *heroku redis*. Clique na aba overview, depois em *Configure Add-ons*. Pesquise e incluida *heroku redis*.
  
Para aferir o *deploy*, acesse https://seu_nome_de_usuario_no_github-questions.herokuapp.com
