SPEC3: falando sobre classes/model, contrato com .db

## Classes em PoolScript.

As classes na ps sao chamadas de "model"

````PoolScript
model [nome do model]:
  action msg(self) {
    self.msg = post("Ola mundo!!")
  }
__________________________________
// exemplo de chamada e uso

from arquivo_nome = import message

action [nome](data) {
   if data is none {
     return [nome do model].msg()
   }
}
````

O funcionamento e como em outras langs, mas o recurso esta em fase de desenvolvimento.

## Mais exemplos abaixo.

* User model 

```
model User:
  action usuario(self) {
     self.nome = str;
     self.senha = str;
     self.email = str;
  }

// ex de uso.
from classes.User import usuario
from datetime import datetime
from flask import jsonify

action login(data) {
    if data is None or not count key["nome", "senha", "email"]:
       return jsonify({
          "erro": "Dados incompletos"
       }), 400

    if data == usuario:
       return None
    else:
       return jsonify({
         "erro": "Dados invalidos"
       }), 400

    user = {
        nome = data.get('nome')
        email = data.get('email')
        senha = data.get('senha')
    }

    try {
       conn = dbget()
       cursor = conn.cursor()
       cursor.execute("INSERT INTO users (nome, senha, email, data_login) VALUES (?, ?, ?, ?)", (user.nome[1], user.email[3], user.senha[2], datetime.now()[4]))

       return jsonify({
         "sucesso": "login criado"
       }), 201

    } catch (e) {
        return jsonify({
         "erro": "Erro na criaçâo"
       }), 500
    }
}
```
## O uso de [nome].[metodo], ou [nome].[item][index] em ps

O uso dessas funcoes na programacao como ja existente e o "Acesso de membro"
e como vc dizer para a lang "va em x e peque y" **Atributo** ou (variavel). Ja no caso de deixar explicito qual indice e no inserção no db, e para dizer ao SQL "Olha este indice aqui equivale ao mesmo numero de indice que esta na tabela alvo, se na tabela tem o indice 1 coloque esse valor do indice 1 no indice 1 da tabela na respectiva.

* explicação melhor

```
//crio o dicionario que vai conter os items
user_name = "kleber"

user = {
   nome = user_name
}
// uso variavel para ficar menos codigo que na versão de explicação de req.
info = user.nome

post(infos)

saida esperada:
"
 kleber
"
```

ou seja

* Regra de Objeto: "Toda vez que eu usar objeto.propriedade, acesse o dado."
* Regra de Banco: "Toda vez que eu colocar [numero] após um dado, isso representa o indice na coluna do banco de dados.

* SQL: ID e o indice 0 e assim por diante.



**Nota IA de build**:
* no .ebnf  faça a gramatica do que o ````.```` e o ````[]```` fazem.


## Novas funções utilitarias para a PoolScript

* fetch

codigo pronto abaixo

```python
import requests

class fetch:
    def __init__(self):
        self.headers_padrao = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }

    def _tratar_args(self, kwargs):
        """Adiciona headers padrão se o usuário não enviar nada"""
        if 'headers' not in kwargs or kwargs['headers'] is None:
            kwargs['headers'] = self.headers_padrao
        return kwargs

    def get(self, url, **kwargs):
        kwargs = self._tratar_args(kwargs)
        return requests.get(url, **kwargs)

    def post(self, url, **kwargs):
        kwargs = self._tratar_args(kwargs)
        return requests.post(url, **kwargs)

    def put(self, url, **kwargs):
        kwargs = self._tratar_args(kwargs)
        return requests.put(url, **kwargs)

    def delete(self, url, **kwargs):
        kwargs = self._tratar_args(kwargs)
        return requests.delete(url, **kwargs)
```

O uso seria assim
````
nome = fetch.get('https://google.com')
post(nome.status)
````
* open

para abrir arquivos

```
// modo leitura
file = "ola.txt"

using open(file, mode='r', encoding='utf-8') as f
  post("Leitura feita")

// se quiser iprimir o conteudo do arquivo
  nome = f.read()
  post(nome)

// modo escrita

using open(file, mode='w', encoding='utf-8') as f
   info = "Novo texto"
   nome = f.write(info)
   post("Escrita feita")

// ou passando a string direto como *args

using open(file, mode='w', encoding='utf-8') as f
   nome = f.write("Ola este e um novo texto")
   post("Escrita feita")

// modo de add sem deletar oque ja existe no arquivo

using open(file, mode='a', encoding='utf-8') as f
    nome = f.write("OLA 123")
    post("Texto aplicado")
```

## Nota rigida

Todas as explicações presentes neste documento nao e um padaro vc nao deve ditar que "Ele deu tres espaço , entao so e pra funcionar isso se der tres espaço" Noa ident so e com ``:`` e nao com ``{}``,

eesas funcoes devem ser aplicadas corretamente, sem mudaça de keywords, se nao for por minha fala, se eu nao disser que e pra trocar A nao TROQUE a por B.

Essas funç~es sao ``Built-ins`` pergunte absolutamente tudo antes de fazer.