Veremos os principais tipos de widgets implementados por padrão no Flutter

## Widgets de layout

São widgets que servem para "organizar" os elementos na tela, ou seja são os widgets que controlam o layout do aplicativo. A seguir veja alguns exemplos de widgets de layout

### Column
Posiciona seus widgets filhos na `vertical`
### Row 
Posiciona seus widgets filhos na `horizontal`

![[column-row.png]]

### Container 
Combina widgets em um container. Pode ser usado para armazenar um ou mais
widgets e depois posicionar este container conforme a necessidade. Um container
básico pode conter: 
- Margem
- Bordar
- Padding

![[container.png]]

### Stack 
Empilha vários widgets, é útil se você quer sobrepor vários widgets criando um "pilha" 
de elementos

![[stack.png]]

### Center
Centraliza o widget filho ao centro

![[center.png]]

### Outros widgets de layout
Veja todos os widgets de layout em: https://docs.flutter.dev/ui/widgets/layout

## Widgets de interação 

São os widgets que permitem a interação entre o usuário e o aplicativo. A seguir veja alguns exemplos de widgets de interação.

### TextField
Um campo de texto que permite que o usuário escreva

![[textfield.png]]

### ElevatedButton
Botão elevado no estilo do material design

![[elevated-button.png]]

### ListView

## Widgets de personalização 

Permitem a personalização dos elementos do aplicativo

### Padding 

`Preenche` um widget ou um conjunto de widgets com um espaço vazio

![[padding.png]]

### Margin 

Adiciona espaços em branco `ao redor` de um widget ou conjunto de widgets.
Utilizamos como um propriedade em determinados widgets.


![[margin.png]]



## Prática

Agora vamos praticar o que aprendemos nessa sessão.

### Criando projeto

Relembrando, utilize este comando para criar um projeto

``` bash
flutter create widgets
```

Agora, acessando a classe `main.dart` remova todo o código para começarmos do zero. Em seguida copie o código.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Demo'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: const Text('Hello World'),
    );
  }
}
```


### Exemplo Column

Vamos começar demonstrando o uso de um widget Column. 
Adicionamos uma Column na propriedade `body`

#### Column

```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(),
    );
  }
}
```

Se você executar o código percebemos que nada acontece. Agora precisamos adicionar widgets a esta "coluna" para que possamos ver o efeito do widget.

Primeiro vamos criar um container que será um quadrado com uma mensagem no seu
interior.

```dart
     Container(
        width: 200,
        height: 200,
	    decoration: BoxDecoration(color: Colors.blue),
        alignment: Alignment.center,
        child: const Text('Primeiro'),
    )
```

Para adicionar widgets a uma Column, utilizamos a propriedade children que nos 
permite adicionar vários "filhos" a um widget

```dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(
        children: [
          Container(
            width: 200,
            height: 200,
            decoration: BoxDecoration(color: Colors.blue),
            alignment: Alignment.center,
            child: const Text('Primeiro'),
          )
        ],
      ),
    );
  }
}

```

Adicione mais alguns Containers 

> [!NOTE] Lembre-se
> utilize vírgula para separar os widgets filhos de Column


```dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(
        children: [
          Container(
            width: 200,
            height: 200,
            decoration: BoxDecoration(color: Colors.blue),
            alignment: Alignment.center,
            child: const Text('Primeiro'),
          ),
          Container(
            width: 200,
            height: 200,
            decoration: BoxDecoration(color: Colors.red),
            alignment: Alignment.center,
            child: const Text('Segundo'),
          ),
          Container(
            width: 200,
            height: 200,
            decoration: BoxDecoration(color: Colors.green),
            alignment: Alignment.center,
            child: const Text('Terceiro'),
          )
        ],
      ),
    );
  }
}

```

Com isso teremos o seguinte resultado 

![[exemplo-column.png]]

Assim percebemos que o uso do widget Column funcionou!

Agora, como fazemos para centralizar esses elementos? 

#### Align 

Para que possamos alinhar todos os widgets criados vamos "envolver" nossa Column
com outro widget chamado `Align`, este widget permite alinhar o seu widget filho.

Utilizamos a propriedade alignment disponível no widget Align, nela atribuímos o valor
Alignment.center que define que o widget será alinhado ao centro

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Align(
        alignment: Alignment.center,
        child: Column(
          children: [
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(color: Colors.blue),
              alignment: Alignment.center,
              child: const Text('Primeiro'),
            ),
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(color: Colors.red),
              alignment: Alignment.center,
              child: const Text('Segundo'),
            ),
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(color: Colors.green),
              alignment: Alignment.center,
              child: const Text('Terceiro'),
            )
          ],
        ),
      ),
    );
  }
}

```



![[alinhamento-horizontal.png]]

Percebemos que o widget foi alinhado horizontalmente apenas, e para alinhar na vertical como fazemos? 

Adicionamos as propriedade mainAxisAlignment  e crossAxisAlignment assim tornando o widget centralizado por completo

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Align(
        alignment: Alignment.center,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(color: Colors.blue),
              alignment: Alignment.center,
              child: const Text('Primeiro'),
            ),
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(color: Colors.red),
              alignment: Alignment.center,
              child: const Text('Segundo'),
            ),
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(color: Colors.green),
              alignment: Alignment.center,
              child: const Text('Terceiro'),
            )
          ],
        ),
      ),
    );
  }
}

```



![[alinhamento-completo.png]]

### Exemplo Row

Vimos um exemplo com Column, agora vamos transformar isso em uma linha
para isso vamos utilizar o widget Row

#### Row
Primeiro vamos criar apenas uma Row

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Row(),
    );
  }
}
```

Como esperado ainda não conseguimos visualizar nada, então vamos criar um 
Container para cada elemento

``` dart
          Container(
            decoration: BoxDecoration(color: Colors.blue),
            child: const Text('Primeiro'),
          )
```


![[texto-sem-padding.png]]

Podemos adicionar um widget de padding ao texto para fazer uma melhoria visual

``` dart 
          Container(
            decoration: BoxDecoration(color: Colors.blue),
            child: Padding(
              padding: const EdgeInsets.all(18.0),
              child: const Text('Primeiro'),
            ),
          )
```

Agora vamos criar mais dois Containers e adicioná-los ao atributo children do widget
Row

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Row(
        children: [
          Container(
            decoration: BoxDecoration(color: Colors.blue),
            child: Padding(
              padding: const EdgeInsets.all(18.0),
              child: const Text('Primeiro'),
            ),
          ),
          Container(
            decoration: BoxDecoration(color: Colors.red),
            child: Padding(
              padding: const EdgeInsets.all(18.0),
              child: const Text('Segundo'),
            ),
          ),
          Container(
            decoration: BoxDecoration(color: Colors.green),
            child: Padding(
              padding: const EdgeInsets.all(18.0),
              child: const Text('Terceiro'),
            ),
          ),
        ],
      ),
    );
  }
}
```



![[row-sem-space.png]]

Podemos também aplicar o conceito de Align para alinhar nossos widgets 

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Align(
        alignment: Alignment.center,
        child: Row(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            Container(
              decoration: BoxDecoration(color: Colors.blue),
              child: Padding(
                padding: const EdgeInsets.all(18.0),
                child: const Text('Primeiro'),
              ),
            ),
            Container(
              decoration: BoxDecoration(color: Colors.red),
              child: Padding(
                padding: const EdgeInsets.all(18.0),
                child: const Text('Segundo'),
              ),
            ),
            Container(
              decoration: BoxDecoration(color: Colors.green),
              child: Padding(
                padding: const EdgeInsets.all(18.0),
                child: const Text('Terceiro'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```


> [!NOTE] Atenção! 
> Note que usamos MainAxisAlignment.spaceEvenly para tornar o layout responsivo

![[row-centralizada.png]]

### Exemplo Stack 
Agora vamos ver um exemplo utilizando o widget Stack

#### Stack 

> [!NOTE] Relembrando
>O widget Stack permite "empilhar" elementos 

Vamos começar criando uma Stack vazia

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Stack(),
    );
  }
}
```

Na Stack temos acesso ao atributo children onde adicionamos "filhos" ao widget. 
Sendo assim vamos criar um Container com uma cor como primeiro elemento. 

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Stack(
        children: [
          Container(
            width: 300,
            height: 300,
            color: Colors.green,
          )
        ],
      ),
    );
  }
}
```



![[stack-verde.png]]

Agora vamos sobrepor o quadrado verde com um outro quadrado menor e com 
outra cor

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Stack(
        children: [
          Container(
            width: 300,
            height: 300,
            color: Colors.green,
          ),
          Container(
            width: 200,
            height: 200,
            color: Colors.blue,
          ),
        ],
      ),
    );
  }
}
```


![[stack-verde-azul.png]]

Podemos adicionar quantos elementos quisermos

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Stack(
        children: [
          Container(
            width: 300,
            height: 300,
            color: Colors.green,
          ),
          Container(
            width: 200,
            height: 200,
            color: Colors.blue,
          ),
          Container(
            width: 100,
            height: 100,
            color: Colors.red,
          ),
        ],
      ),
    );
  }
}
```


![[stack-verde-azul-vermelho.png]]

Se quisermos posicionar os elementos dentro de uma Stack podemos utilizar
o widget Positioned e adicionar nosso Container como filho

``` dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Stack(
        children: [
          Container(
            width: 300,
            height: 300,
            color: Colors.green,
          ),
          Positioned(
            left: 100,
            child: Container(
              width: 200,
              height: 200,
              color: Colors.blue,
            ),
          ),
          Container(
            width: 100,
            height: 100,
            color: Colors.red,
          ),
        ],
      ),
    );
  }
}
```



![[positioned.png]]


Outro exemplo 

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Stack(
        children: [
          Container(
            width: 300,
            height: 300,
            color: Colors.green,
          ),
          Positioned(
            left: 100,
            child: Container(
              width: 200,
              height: 200,
              color: Colors.blue,
            ),
          ),
          Positioned(
            left: 70,
            child: Container(
              width: 100,
              height: 100,
              color: Colors.red,
            ),
          ),
        ],
      ),
    );
  }
}
```


![[positioned-vermelho.png]]


### Exemplo criando um Formulário
Agora vamos criar um Formulário com o widget Form

#### Form

Criamos um form básico utilizando como child um widget Column pois
os elementos interiores serão posicionados verticalmente. 

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Form(
          child: Column(
        children: [
          TextFormField(),
          TextFormField(),
          ElevatedButton(onPressed: () {}, child: const Text('enviar'))
        ],
      )),
    );
  }
}
```


![[Pasted image 20250329201627.png]]

Podemos então envolver o widget Form em um Container para que possamos 
dimensionar o seu tamanho, também envolvemos o widget Container em 
um widget Center para fazer a centralização

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          width: 300,
          height: 300,
          decoration: BoxDecoration(
              border: Border.all(color: Colors.blue),
              borderRadius: BorderRadius.circular(12)),
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Form(
                child: Column(
              children: [
                TextFormField(),
                TextFormField(),
                ElevatedButton(onPressed: () {}, child: const Text('enviar'))
              ],
            )),
          ),
        ),
      ),
    );
  }
}
```

![[Pasted image 20250329202354.png]]

Agora podemos personalizar os campos de texto utilizando labels e 
adicionando um espaço entre cada elemento

```dart 
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          width: 300,
          height: 300,
          decoration: BoxDecoration(
              border: Border.all(color: Colors.blue),
              borderRadius: BorderRadius.circular(12)),
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Form(
                child: Column(
              children: [
                TextFormField(
                  decoration: InputDecoration(labelText: 'Nome'),
                ),
                const SizedBox(
                  height: 20,
                ),
                TextFormField(
                  decoration: InputDecoration(labelText: 'Sobrenome'),
                ),
                const SizedBox(
                  height: 40,
                ),
                ElevatedButton(onPressed: () {}, child: const Text('enviar'))
              ],
            )),
          ),
        ),
      ),
    );
  }
}
```


![[Pasted image 20250329202726.png]]

Agora para tornar o Form funcional devemos criar e adicionar Controllers a cada campo.

> [!NOTE] Controllers 
> são atributos adicionados a cada campo que permitem o `acesso` ao
valor de cada campo

``` dart
class _MyHomePageState extends State<MyHomePage> {
  final _firstNameController = TextEditingController();
  final _lastNameController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          width: 300,
          height: 300,
          decoration: BoxDecoration(
              border: Border.all(color: Colors.blue),
              borderRadius: BorderRadius.circular(12)),
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Form(
                child: Column(
              children: [
                TextFormField(
                  decoration: InputDecoration(labelText: 'Nome'),
                  controller: _firstNameController,
                ),
                const SizedBox(
                  height: 20,
                ),
                TextFormField(
                  decoration: InputDecoration(labelText: 'Sobrenome'),
                  controller: _lastNameController,
                ),
                const SizedBox(
                  height: 40,
                ),
                ElevatedButton(onPressed: () {}, child: const Text('enviar'))
              ],
            )),
          ),
        ),
      ),
    );
  }
}
```

Com os controllers criados agora podemos criar uma função e chamá-la no `onpressed` do nosso botão. Nesta função printamos os nomes através do controller

``` dart
class _MyHomePageState extends State<MyHomePage> {
  final _firstNameController = TextEditingController();
  final _lastNameController = TextEditingController();

  void submit() {
    debugPrint(_firstNameController.text);
    debugPrint(_lastNameController.text);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          width: 300,
          height: 300,
          decoration: BoxDecoration(
              border: Border.all(color: Colors.blue),
              borderRadius: BorderRadius.circular(12)),
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Form(
                child: Column(
              children: [
                TextFormField(
                  decoration: InputDecoration(labelText: 'Nome'),
                  controller: _firstNameController,
                ),
                const SizedBox(
                  height: 20,
                ),
                TextFormField(
                  decoration: InputDecoration(labelText: 'Sobrenome'),
                  controller: _lastNameController,
                ),
                const SizedBox(
                  height: 40,
                ),
                ElevatedButton(
                    onPressed: () => submit(), child: const Text('enviar'))
              ],
            )),
          ),
        ),
      ),
    );
  }
}
```

Agora que conseguimos printar os nomes, devemos adicionar uma validação aos 
campos, isso é feito através do atributo validator no campo.


> [!NOTE] Validator
> No validator adicionamos uma função que verifica o texto atual no campo e retorna uma mensagem de erro caso necessário

```dart
                TextFormField(
                    decoration: InputDecoration(labelText: 'Nome'),
                    controller: _firstNameController,
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Por favor insira o seu nome';
                      }
                      return null;
                    }),
                const SizedBox(
                  height: 20,
                ),
                TextFormField(
                    decoration: InputDecoration(labelText: 'Sobrenome'),
                    controller: _lastNameController,
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Por favor insira o seu sobrenome';
                      }
                      return null;
                    }),

```

Ainda é necessário fazer uma alteração, devemos criar um identificador do formulário e
na nossa função `submit` fazer uma nova verificação

```dart
class _MyHomePageState extends State<MyHomePage> {
  final _formKey = GlobalKey<FormState>();
  final _firstNameController = TextEditingController();
  final _lastNameController = TextEditingController();

  void submit() {
    if (_formKey.currentState?.validate() ?? false) {
      ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Cadastro realizado com sucesso!')));
      debugPrint(_firstNameController.text);
      debugPrint(_lastNameController.text);

      _firstNameController.text = '';
      _lastNameController.text = '';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          width: 300,
          height: 300,
          decoration: BoxDecoration(
              border: Border.all(color: Colors.blue),
              borderRadius: BorderRadius.circular(12)),
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Form(
                key: _formKey,
                child: Column(
                  children: [
                    TextFormField(
                        decoration: InputDecoration(labelText: 'Nome'),
                        controller: _firstNameController,
                        validator: (value) {
                          if (value == null || value.isEmpty) {
                            return 'Por favor insira o seu nome';
                          }
                          return null;
                        }),
                    const SizedBox(
                      height: 20,
                    ),
                    TextFormField(
                        decoration: InputDecoration(labelText: 'Sobrenome'),
                        controller: _lastNameController,
                        validator: (value) {
                          if (value == null || value.isEmpty) {
                            return 'Por favor insira o seu sobrenome';
                          }
                          return null;
                        }),
                    const SizedBox(
                      height: 40,
                    ),
                    ElevatedButton(
                        onPressed: () => submit(), child: const Text('enviar'))
                  ],
                )),
          ),
        ),
      ),
    );
  }
}
```

A função `submit` agora só é executada caso nenhum erro seja retornado em algum 
validator pertencente ao form com a key especificada. 