# Single Responsibility Principle

__Uma classe deve ter apenas uma única responsabilidade__

Caso nossas classes assumirem múltiplas responsabilidades (com vários métodos implementados de objetivos diferentes), elas se 
tornarão exageradamente acopladas e com muitas funcionalidades, tornando-as mais dificeis de manter.

Considere o trecho de código a seguir:

```ts
class Aluno {
    public criarContaDoAluno() {
        // Lógica para criar conta do aluno
    }

    public calcularNotaDoAluno() {
        // Lógica para o cálculo da nota
    }

    public gerarDadosDoAluno() {
        // Lógica para gerar dados do aluno
    }
}
```

A classe ```Aluno``` no código acima viola o SRP. Note que, os métodos contidos nela definem ações distintas como 
"criar uma conta" ou "calcular nota". Para solucionar essa barreira, deveríamos dividir a classe ```Aluno``` em outros estados
de responsabilidade.

```ts
class CriarConta {
    public criarContaDoAluno() {
        // Lógica para criar conta do aluno
    }
}

class CalcularNota {
    public calcularNotaDoAluno() {
        // Lógica para calcular nota do aluno 
    }
}

class GerarDados {
    public gerarDadosDoAluno() {
        // Lógica para gerar dados do aluno
    }
}
```

Agora que dividimos as responsabilidades, cada classe tem apenas um dever, uma funcionalidade e apenas uma 
alteração que precisa ser feita. Dessa forma nosso código fica mais simples de explicar e compreender.

Esta forma de diferenciar responsabilidades fica mais evidente quando tratamos de um caso mais geral, em uma API Rest 
por exemplo, quando temos que separar uma entidade ```User``` em "pequenos pedaços": ```UserEntity```, ```UserController```, ```UserRespository``` e mais.

