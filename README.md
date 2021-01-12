# SOLID em PT-BR
## Este é um repositório para mostrar o funcionamento dos princípios SOLID e de como devemos escrever código com qualidade.

Esses principios foram concebidos pela primeira vez por __Robert C. Martin__ em seu artigo do ano 2000, [Design Principles and Design Patterns](https://fi.ort.edu.uy/innovaportal/file/2032/1/design_principles.pdf).
Esses conceitos foram construidos posteriormente por __Michael Feathers__, quem nos introduziu o acrônimo SOLID.

Seguindo esses 5 conceitos temos:

- __S__ Single Responsibility
- __O__ Open/Closed
- __L__ Liskov Substituition
- __I__ Interface Segregation
- __D__ Dependency Inversion

Vamos explicar a fundo e na prática usando diversos exemplos sobre cada um deles e o que eles significam.

## Single Responsibility (Responsabilidade Única)

Começando com o primeiro princípio do acrônimo, a responsabilidade única afirma que _uma classe deveria somente ter uma única responsabilidade_.

E como isso poderia nos ajudar na construção de um software melhor ?

- Uma classe com uma única responsabilidade terá menos casos de testes.

- Menos funcionalidade em uma única classe terá menos dependência.

- Melhora na organização, classes menores e bem organizadas ficam mais fáceis de encontrar em um projeto.

Vamos ter uma melhor ideia disto na prática.

```php
class Pessoa
{
    protected $nome;
    protected $idade;
    protected $altura;

    public function __construct($nome, $idade, $altura)
    {
        $this->nome = $nome;
        $this->idade = $idade;
        $this->altura = $altura;
    }

    public function verificaMaioridade()
    {
        if ($this->idade < 18) {
            echo "$this->nome é menor de idade";
        } else {
            echo "$this->nome é maior de idade";
        }
    }

    /*
     * Ações
     * 
     */

    public function andar()
    {
        echo "$this->nome está andando";
    }

    public function correr()
    {
        echo "$this->nome está correndo";
    }
} 
```

Esse código funciona sem nenhum erro, porém, viola o princípio. Note que métodos foram adicionados que não necessariamente 'combinam' com a classe. Poderiamos resolver ou melhorar isso criando uma nova classe __PessoaAction__ para definir todas as ações que a pessoa pode ter.

```php
class PessoaAction extends Pessoa
{
    public function andar()
    {
        echo "$this->nome está andando";
    }

    public function correr()
    {
        echo "$this->nome está correndo";
    }

    public function pular()
    {
        echo "$this->nome está pulando";
    }
} 
```

Podemos aproveitar essa nova classe criada adicionar todos os métodos (ações no caso) que a classe mãe pode requirir, assim na medida que o projeto crescer, será muito mais fácil identificar o que cada arquivo de classe deve fazer.
