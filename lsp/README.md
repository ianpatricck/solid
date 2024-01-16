# Liskov Substitution Principle 

__Um objeto que utilize uma classe base deve continuar a funcionar corretamente quando um objeto derivado 
dessa classe base for passado para ele.__

O princípio de Liskov trabalha com o uso de classes abstratas ou interfaces com a ideia de 
criar um "contrato" entre classes.

Liskov diz que se uma classe B herda de uma classe A, então a classe B deve se comportar como a classe A.
A é a classe base e deve responder por todas as classes que herdam dela.

Observe o sistema abaixo:

```ts
interface IUsuario {
    criarUsuario(): boolean;
    deletarUsuario(): boolean;
    logar(): boolean;
}
```

Nossa interface ```IUsuario``` funciona como nosso contrato com todos os métodos que devem ser implementados.

```ts
class Membro implements IUsuario {
    criarUsuario(): boolean {
        // ...
    }

    deletarUsuario(): boolean {
        // ... 
    }

    logar(): boolean {
        // ... 
    }
}
```

A classe ```Membro``` implementa todos os métodos da interface ```IUsuario```.


```ts
class Administrador implements IUsuario {
    criarUsuario(): boolean {
        // ...
    }

    deletarUsuario(): boolean {
        // ... 
    }

    logar(): boolean {
        // ... 
    }
}
```

A classe ```Administrador``` também implementa todos os métodos da interface ```IUsuario```.
Muitas outras classes poderiam ser criadas e implementadas individualmennte com os métodos da interface de usuário.

```ts
class Sistema {
    entrar(usuario: IUsuario): string { 
        const usuarioLogado = usuario.logar();

        if (usuarioLogado == true)
            return "Usuário logado com sucesso!";
        else
            return "Falha ao autenticar no sistema!";
    }
}

const usuario = new Membro();
const sistema = new Sistema();

const resposta = sistema.entrar(usuario);
console.log(resposta);
```

Perceba que qualquer objeto que implementa ```IUsuario``` será compátivel com o 
método ```entrar``` da classe ```Sistema```, pois o método ```logar``` sempre retorna um valor booleano (true ou false).
Se outra classe herdar de ```Membro``` ou ```Administrador```, ela também terá o mesmo comportamento quando passada
como argumento ao método ```entrar``` da classe ```Sistema```, já que seus métodos terão a mesma assinatura 
dos métodos ma interface ```IUsuario```.

Observe que se removermos a exigência de retornar um valor booleano no método ```logar``` da interface ```IUsuario```
teriamos problemas com o login do usuário, pois cada tipo de usuário (classes que implementam a interface) tem sua própria
implementação e sempre deveria retornar ```true``` ou ```false```.

