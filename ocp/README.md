# Open/Closed Principle

__Classes devem ser abertas para extensão, mas fechadas para modificação__

Este príncipio defende a ideia de que as classes devem ser ampliadas, mas não modificadas. Quando editamos classes
existentes, corremos o risco de quebrarmos o código que já foi testado e aprovado.
A vantagem dessa métodologia é que você pode adicionar novas features sem modificar o código antigo. Dessa forma, você
não quebrará as implementações das classes originais.

Sejam duas classes ```Triangulo``` e ```Retangulo```:

```ts
class Triangulo {
  public base: number;
  public altura: number;

  constructor(base: number, altura: number) {
    this.base = base;
    this.altura = altura;
  }
}

class Retangulo {
  public largura: number;
  public altura: number;
  constructor(largura: number, altura: number) {
    this.largura = largura;
    this.altura = altura;
  }
}
```

Agora imagine que queremos desenvolver uma função que calcule a área destas figuras geométricas:

```ts
function calcularAreaDaFigura(figura: Retangulo | Triangulo): number {
    let resultado = 0;
    
    if (figura instanceof Retangulo) {
        resultado = figura.largura * figura.altura;
    }
 
    if (figura instanceof Triangulo) {
        resultado = (figura.base * figura.altura) / 2;
    }

    return resultado;
}
```

O problema com essa função é que para cada nova figura geométrica que criamos, precisamos mudar a implementação, Adicionando
um novo ```if```. 

Podemos resolver esse problema com uma interface que contenha um método para calcular a área:

```ts
interface AreaDaFiguraInterface {
  calcularArea(): number;
}
```

Agora as classes de figuras deverão implementar nossa nova interface para ter o método de calcular a área.

```ts
class Triangulo implements AreaDaFiguraInterface {
  public base: number;
  public altura: number;

  constructor(base: number, altura: number) {
    this.base = base;
    this.altura = altura;
  }

  public calcularArea(): number {
    return (this.base * this.altura) / 2;
  }
}

class Retangulo implements AreaDaFiguraInterface {
  public largura: number;
  public altura: number;

  constructor(largura: number, altura: number) {
    this.largura = largura;
    this.altura = altura;
  }

  public calcularArea(): number {
    return this.largura * this.altura;
  }
}
```

Agora podemos também mudar nossa função ```calcularAreaDaFigura()``` da seguinte forma:

```ts
function calcularAreaDaFigura(figura: AreaDaFiguraInterface): number => figura.calcularArea();
```

