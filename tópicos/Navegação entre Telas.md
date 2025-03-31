Aplicativos mais complexos necessitam de um sistema de navegação para que seja
possível transitar entre telas. No flutter isso é possível através do uso do `Navigator`

## Navigator 
É um widget que permite "navegar" entre diferentes telas. Funciona de forma similar a 
uma Stack, com isso temos os seguintes métodos: 

- Navigator.push(): empilha uma tela na pilha e redireciona para a tela
- Navigator.pop(): desempilha a tela e volta para a tela anterior 

![[navigator.png]]

### Rotas nomeadas

É possível nomear uma rota para facilitar o seu uso, porém isso causa uma leve perda
de performance, então devemos utilizar somente quando estamos trabalhando com
um projeto pequeno que não demande tal performance.

#### Exemplo prático de rotas nomeadas

Podemos definir rotas previamente na função MaterialApp que é retornada na classe 
MyApp. 

```dart
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
      initialRoute: '/',
      routes: {'/': (context) => MyHomePage(title: 'Demo')},
    );
  }
}
```

Removemos o atributo home e adicionamos um atributo para a rota inicial e um para 
todas as rotas presentes no aplicativo (no momento só temos uma).

Agora na tela principal vamos adicionar botões para trocar entre telas

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
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            ElevatedButton(
                onPressed: () {
                  Navigator.pushNamed(context, '/secundaria-a');
                },
                child: const Text('Lado A')),
            const SizedBox(
              height: 20,
            ),
            ElevatedButton(
                onPressed: () {
                  Navigator.pushNamed(context, '/secundaria-b');
                },
                child: const Text('Lado B')),
          ],
        ),
      ),
    );
  }
}
```


![[Pasted image 20250331165548.png]]


Repare que ainda não adicionamos as rotas de fato ao atributo routes para isso 
precisamos primeiro criar as classes 

**secundaria_a.dart**
```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class SecundariaA extends StatelessWidget {
  const SecundariaA({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.symmetric(vertical: 30.0),
          child: Column(
            children: [
              Text(
                'Secundária A',
                style: TextStyle(
                  fontSize: 32,
                  fontWeight: FontWeight.bold,
                  color: Colors.black,
                ),
              ),
              const SizedBox(
                height: 20,
              ),
              SizedBox(
                width: 220,
                child: ElevatedButton(
                    onPressed: () {
                      Navigator.pushNamed(context, '/secundaria-b');
                    },
                    child: const Text('Secundária B')),
              ),
              const SizedBox(
                height: 20,
              ),
              SizedBox(
                width: 220,
                child: ElevatedButton(
                    onPressed: () {
                      Navigator.popUntil(context, ModalRoute.withName('/'));
                    },
                    child: const Text('Voltar para tela principal')),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

**secundaria_b.dart**
```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class SecundariaB extends StatelessWidget {
  const SecundariaB({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.symmetric(vertical: 30.0),
          child: Column(
            children: [
              Text(
                'Secundária B',
                style: TextStyle(
                  fontSize: 32,
                  fontWeight: FontWeight.bold,
                  color: Colors.black,
                ),
              ),
              const SizedBox(
                height: 20,
              ),
              SizedBox(
                width: 220,
                child: ElevatedButton(
                    onPressed: () {
                      Navigator.pushNamed(context, '/secundaria-a');
                    },
                    child: const Text('Secundária A')),
              ),
              const SizedBox(
                height: 20,
              ),
              SizedBox(
                width: 220,
                child: ElevatedButton(
                    onPressed: () {
                      Navigator.popUntil(context, ModalRoute.withName('/'));
                    },
                    child: const Text('Voltar para tela principal')),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Utilizamos o método **pushNamed** para navegar para uma rota através do seu nome, e 
utilizamos o método **popUntil** para "voltar" para a tela principal removendo as outras
telas.

Agora no arquivo **main.dart** adicionamos as rotas no atributo **routes**

```dart
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
      initialRoute: '/',
      routes: {
        '/': (context) => MyHomePage(title: 'Demo'),
        '/secundaria-a': (context) => SecundariaA(),
        '/secundaria-b': (context) => SecundariaB(),
      },
    );
  }
}
```