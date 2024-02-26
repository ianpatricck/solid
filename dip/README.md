# Dependency Inversion Principle

__Módulos de alto nível não deveriam depender de módulos de baixo nível. Ambos deveriam depender de abstrações.__

Este princípio tem a função de favorecer o desacoplamento entre os níveis dos módulos.
Vamos fixar esses conceitos com exemplos.

Nos trechos de código abaixo temos uma simulação de banco de dados, no qual nosso objetivo é que possamos usar e alterar para qualquer tipo de banco (PostgreSQL, MySQL, MongoDB e etc...) sem modificar o nosso código de alto nível. 

Interface usada como "espelho" que será implementada pelo código de baixo nível.
```ts
interface BancoDeDadosInterface {
    public conectar();
    public resgatar();
    public inserir(dados: object);
    public deletar(id: string);
}
```

Classe ```PostgresSQLDatabase``` de baixo nível que implementa nossa interface "espelho".

```ts
class PostgresSQLDatabase implements BancoDeDadosInterface {
    public conectar() { /* ... */ }
    public resgatar() { /* ... */ }
    public inserir(dados: object) { /* ... */ }
    public deletar(id: string) { /* ... */ }
}
```

Uma classe genérica de alto nível ```Database``` que vai usar a interface ```BancoDeDadosInterface```. Ao instanciarmos esta classe, podemos usar os métodos da classe ```PostgresSQLDatabase``` caso ela seja passada no construtor.

```ts
class Database {
    private database: BancoDeDadosInterface;
    
    constructor(database) {
        this.database = database;
    }
    
    public inserirDado(dado: object): string {
        this.database.inserir(dado);
        return "Dado inserido com sucesso!";
    }
}
```

Um simples script que mostra como seria a inserção de um dado usando a classe ```Database```.

```ts
const banco = new Database(new PostgresSQLDatabase());
const conexao = banco.conectar();

const dadoFoiInserido = conexao.inserirDado({ 
    nome: "John", 
    sobrenome: "Smith"
});

console.log(dadoFoiInserido);
```

Poderiamos testar outro banco de dados. Para isso, deveriamos criar outra classe de baixo nível que implementa nossa interface espelho.

Vamos criar nossa classe chamada ```MySQLDatabase```.

```ts
class MySQLDatabase implements BancoDeDadosInterface {
    public conectar() { /* ... */ }
    public resgatar() { /* ... */ }
    public inserir(dados: object) { /* ... */ }
    public deletar(id: string) { /* ... */ }
}
```

Agora é só substituir nosso script usando essa classe.

```ts
const banco = new Database(new MySQLDatabase());
const conexao = banco.conectar();

const dadoFoiInserido = conexao.inserirDado({ 
    nome: "John", 
    sobrenome: "Smith"
});

console.log(dadoFoiInserido);
```

### Considerações finais

Esse é um modelo de como o ```DIP``` protege o nosso código de alto nível para que ele não sofra modificações. Os exemplos apresentados são apenas para fins didáticos e de compreensão do princípio. Isso não quer dizer que a configuração de banco de dados deve seguir esses passos (na verdade não deve!), você pode aplicar esse esquema para treinar e relacionar em outros contextos. 
