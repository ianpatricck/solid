# Interface Segregation Principle

__Nenhum cliente deve ser forçado a depender de interfaces que não usa__

Interfaces grandes, que possuem muitos métodos a serem implementados ou até mesmo que não serão utilizados ou que nada tem haver com o comportamento da interface, geralmente ocasionam em complexidade desnecessária e acabam ocupando espaço dentro do projeto.

Veja um exemplo:

```ts
interface IUsuario {
	public cadastrar();
	public atualizar();
	public excluirConta();
	public cadastrarUsuario(id: string);
	public excluirUsuario(id: string);
	public banirUsuario(id: string);
}
```

Como pode-se observar na interface ```IUsuario```, temos métodos de um usuário comum do sistema, mas também temos métodos de usuário com maiores privilégios, como um administrador. Não queremos que um usuário genérico seja capaz de banir ou excluir outro usuário, certo? Desse modo, a estratégia é decompor nossa interface em duas interfaces separando esses métodos.

```ts
interface IUsuario {
	public cadastrar();
	public atualizar();
	public excluirConta();
}
```

```ts
interface IUsuarioAdministrador {
	public cadastrar();
	public atualizar();
	public excluirConta();
	public cadastrarUsuario(id: string);
	public excluirUsuario(id: string);
	public banirUsuario(id: string);
}
```

Note agora que a interface ```IUsuario```não possui os métodos de privilégio administrativo.
Agora podemos implementar ambas as interfaces com nossas classes.

```ts
class Usuario implements IUsuario {
	public cadastrar() { /* ... */ }
	public atualizar() { /* ... */ }
	public excluirConta() { /* ... */ }
}
```

```ts
class UsuarioAdministrador implements IUsuarioAdministrador {
	public cadastrar() { /* ... */ }
	public atualizar() { /* ... */ }
	public excluirConta() { /* ... */ }
	public cadastrarUsuario(id: string) { /* ... */ }
	public excluirUsuario(id: string) { /* ... */ }
	public banirUsuario(id: string) { /* ... */ }
}
```
