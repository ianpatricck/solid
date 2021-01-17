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

Vamos ter uma melhor ideia disso na prática.

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
        /* ... */
    }

    public function correr()
    {
        /* ... */
    }
} 
```

Esse código funciona sem nenhum erro, porém, viola o princípio. Note que métodos foram adicionados que não necessariamente 'combinam' com a classe. Poderiamos resolver ou melhorar isso criando uma nova classe __PessoaAction__ para definir todas as ações que a pessoa pode ter.

```php
class PessoaAction extends Pessoa
{
    public function andar()
    {
        /* ... */
    }

    public function correr()
    {
        /* ... */
    }

    public function pular()
    {
        /* ... */
    }
} 
```

Podemos aproveitar essa nova classe criada adicionar todos os métodos (ações no caso) que a classe mãe pode requirir, assim na medida que o projeto crescer, será muito mais fácil identificar o que cada arquivo de classe deve fazer.

## Open/Closed

Simplificando esse princípio, _classes deveriam ser abertas para extensão, mas fechadas para modificação_. Ao fazer isso, paramos de modificar o código existente e causar novos bugs potenciais. Claro, a única exceção à regra é ao consertar bugs no código existente.

Exemplificando:

Imagine que como parte de um novo projeto, nós implementamos uma classe __Carro__.

Ela é bem complexa e com vários atributos e métodos.

```php
class Carro
{
    private $marca;
    private $modelo;
    private $vel;

    // Construtores, getters e setters ...
}
```

Nós lançamos o projeto e todo mundo aprovou. Entretando, depois de poucos meses, nós decidimos que a classe __Carro__ está um pouco 'vazia' e poderiamos adicionar atributos de carros que tenham funcionalidades únicas.

Nesse caso, abrimos a classe __Carro__ e adicionamos esses atributos, porém, quem sabe quais erros isso pode ocasionar em seu projeto ?

Ao invés disso, vamos usar o princípio _Open/Closed_ e simplesmente extender a classe __Carro__.

```php
class CarroRaro extends Carro
{
    private $atributoUnicoDoCarroRaro;

    // Construtores, setters e getters ...
}
```

Por extender nossa classe __Carro__ podemos ter certeza que nosso projeto não será afetado.

## Liskov Substitution

O próximo da lista é __Liskov Substitution__, sem dúvidas é o princípio mais complexo dos outros 5. __Se uma classe A é um subtipo da classe B, então nos deveriamos ter capacidade para substituir B com A sem implicar no comportamento do nosso programa__.

Indo direto ao código:

```php
interface Carro
{
    public function ligarMotor();
    public function acelerar();
}
```

No código acima, definimos uma simples interface chamada __Carro__ com dois métodos que todos os carros deveriam cumprir, ligar o motor e acelerar.

```php
class Automovel implements Carro
{
    private $motor;

    public function __construct(Motor $motor)
    {
        $this->motor = $motor;
    }

    public function ligarMotor()
    {
        $this->motor->ligar();
    }

    public funtion acelerar()
    {
        $this->motor->vel(1000);
    }
}
```

Como nosso código descreve, nós temos um motor que podemos ligar e podemos acelerar, através de métodos de uma classe __Motor__ que passamos por injeção de dependência no construtor. Mas, e se quisermos adicionar um carro elétrico ?

```php
class CarroEletrico implements Carro
{
    public function ligarMotor()
    {
       throw new Exception('Nós não temos um motor');
    }

    public fnuction acelerar()
    {
        // Essa aceleração é uma loucura
    }
}
```

Jogando um carro sem motor dentro de tudo, estamos mudando inerentemente o comportamento do nosso programa. Esta é uma violação flagrante da __substituição de Liskov__ e é um pouco mais difícil de corrigir do que nossos 2 princípios anteriores.

Uma possivel solução deveria ser trabalhar novamente nosso modelo dentro da interface que levam em consideração o estado sem motor do nosso carro.
