## Estado


> [!NOTE] O que é estado? 
> `estado` se refere a qualquer informação que pode mudar durante a execução do aplicativo e afetar a interface do usuário

**Um estado pode ser divido em:** 
1. Estado Local
2. Estado Global 
### Estado Local

É um estado que pertence somente a um widget específico, este estado não precisa
ser compartilhado com outros widgets.
**Ex:** Um botão que muda de cor quando pressionado

Para fazer este controle utilizamos um `StatefulWidget`

#### setState
É uma função de atualização de estado. Que permite atualizar em tempo real o valor
de uma ou mais variáveis para os componentes que a acessam (dentro de uma única 
tela).

#### initState
É uma função que define o estado da tela no momento da sua inicialização.

#### dispose
É uma função chamada quando uma tela é fechada, ela tem como objetivo destruir
objetos que não serão mais utilizado, assim liberando memória.

#### Exemplo prático 

Vamos ver como utilizar setState, initState e dispose em um único exemplo.
A nossa intenção será criar um widget dinâmico que troque um texto conforme
o usuário digite.

A primeira coisa que devemos fazer é implementar as funções initState e dispose.
Criaremos um controller para acessar o texto que será modificado e uma variável 
que ira armazenar o texto.

``` dart 
class _MyHomePageState extends State<MyHomePage> {
  String _myText = 'Escreva algo';
  late TextEditingController _textEditingController;

  @override
  void initState() {
    super.initState();
    setState(() {
      _textEditingController = TextEditingController();
    });
  }

  @override
  void dispose() {
    _textEditingController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: Text(
          _myText,
          style: TextStyle(fontSize: 30),
        ));
  }
}
```


![[Pasted image 20250329225650.png]]

Agora vamos criar um campo de texto que utilizará o nosso controller. Portanto vamos
criar uma SizedBox que servirá com um Container com uma Column para alinhar os
elementos verticalmente.

``` dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: Center(
          child: Padding(
            padding: const EdgeInsets.only(top: 100),
            child: SizedBox(
              width: 300,
              child: Column(
                children: [
                  Text(
                    _myText,
                    style: TextStyle(fontSize: 30),
                  ),
                  const SizedBox(
                    height: 20,
                  ),
                  TextField(
                    controller: _textEditingController,
                    decoration: InputDecoration(
                        border: OutlineInputBorder(), labelText: 'Digite aqui'),
                  )
                ],
              ),
            ),
          ),
        ));
  }
```


![[Pasted image 20250329230507.png]]

Se tentarmos digitar, perceberemos que nada acontece. Isso porque não estamos utilizando o nosso controller corretamente. Devemos adicionar uma função no
TextField chamada onChanged, esta função será executa sempre que ocorrer
alguma mudança no campo de texto.

```dart 
                  TextField(
                    controller: _textEditingController,
                    decoration: InputDecoration(
                        border: OutlineInputBorder(), labelText: 'Digite aqui'),
                    onChanged: (value) {
                      _myText = value.isEmpty ? 'Escreva algo' : value;
                    },
                  )

```

A nossa função ainda não esta funcionando, você sabe por qual motivo? Isso! Não
estamos utilizando o setState.

> [!NOTE] Lembrete
> Sempre que for necessário alterar um estado devemos utilizar o método **setState()**


``` dart
class _MyHomePageState extends State<MyHomePage> {
  String _myText = 'Escreva algo';
  late TextEditingController _textEditingController;

  @override
  void initState() {
    super.initState();
    setState(() {
      _textEditingController = TextEditingController();
    });
  }

  @override
  void dispose() {
    _textEditingController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: Center(
          child: Padding(
            padding: const EdgeInsets.only(top: 100),
            child: SizedBox(
              width: 300,
              child: Column(
                children: [
                  Text(
                    _myText,
                    style: TextStyle(fontSize: 30),
                  ),
                  const SizedBox(
                    height: 20,
                  ),
                  TextField(
                    controller: _textEditingController,
                    decoration: InputDecoration(
                        border: OutlineInputBorder(), labelText: 'Digite aqui'),
                    onChanged: (value) {
                      setState(() {
                        _myText = value.isEmpty ? 'Escreva algo' : value;
                      });
                    },
                  )
                ],
              ),
            ),
          ),
        ));
  }
}
```

Agora nosso código esta funcionando perfeitamente!

![[Pasted image 20250329231459.png]]



### Estado Global
É um estado que precisa ser acessado em diferentes partes do aplicativo.
**Ex:** O tema do aplicativo

Existem várias formas de fazer este controle, as mais utilizadas são: 
- Provider
- Riverpod

#### Exemplo prático Provider

Primeiramente para utilizar o provider devemos importa-lo para isso podemos:

1. Importar automaticamente
```shell
flutter pub add provider
```

2. Importar manualmente no arquivo `pubspec.yaml`
  ```shell
  dependencies:
  provider: ^6.1.4
```

Agora vamos criar uma classe chamada **text_provider.dart**, esta classe será nosso provider

```dart
import 'package:flutter/foundation.dart';

class TextProvider extends ChangeNotifier {
  String _text = 'Escreva algo';
  String get text => _text;

  void updateText(String newText) {
    _text = newText.isEmpty ? 'Escreva algo' : newText;
    notifyListeners();
  }
}
```

Vamos criar um exemplo semelhante ao anterior porém com a adição de uma nova tela
que é acessível através de um botão na tela principal. O texto em ambas telas será 
alterado através dos campos de texto.

Primeiro devemos alterar nossa função runApp para utilizar o nosso provider

```dart
void main() {
  runApp(ChangeNotifierProvider(
      create: (context) => TextProvider(), child: const MyApp()));
}
```

Nossa tela principal será semelhante ao exemplo anterior

``` dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    final textProvider = Provider.of<TextProvider>(context);

    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.only(top: 100.0),
          child: SizedBox(
            width: 300,
            child: Column(
              children: [
                Consumer<TextProvider>(
                    builder: (context, provider, child) => Text(
                          provider.text,
                          style: const TextStyle(
                              fontSize: 30, fontWeight: FontWeight.bold),
                        )),
                const SizedBox(
                  height: 20,
                ),
                TextField(
                  decoration: InputDecoration(
                      border: OutlineInputBorder(), labelText: 'Digite aqui'),
                  onChanged: (value) {
                    textProvider.updateText(value);
                  },
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

Mudanças: 
- Criamos um objeto provider armazenado na variável textProvider;
- Na função **onChanged** do TextField chamamos o método updateText do nosso provider;

![[Pasted image 20250331141252.png]]

Agora vamos criar nossa segunda tela, que será praticamente igual a tela principal

`secundaria.dart`

``` dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:provider_example/text_provider.dart';

class Secundaria extends StatefulWidget {
  const Secundaria({super.key});

  @override
  State<Secundaria> createState() => _SecundariaState();
}

class _SecundariaState extends State<Secundaria> {
  @override
  Widget build(BuildContext context) {
    final textProvider = Provider.of<TextProvider>(context);

    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.only(top: 100.0),
          child: SizedBox(
            width: 300,
            child: Column(
              children: [
                Consumer<TextProvider>(
                    builder: (context, provider, child) => Text(
                          provider.text,
                          style: const TextStyle(
                              fontSize: 30, fontWeight: FontWeight.bold),
                        )),
                const SizedBox(
                  height: 20,
                ),
                TextField(
                  decoration: InputDecoration(
                      border: OutlineInputBorder(), labelText: 'Digite aqui'),
                  onChanged: (value) {
                    textProvider.updateText(value);
                  },
                ),
                const SizedBox(
                  height: 20,
                ),
                ElevatedButton(
                    onPressed: () => Navigator.pop(context),
                    child: const Text('Trocar tela'))
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

Mudanças: 
- Adicionamos um botão que faz a navegação para a tela principal

Agora na tela principal adicionamos um botão que faz a navegação para a tela secundária

```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    final textProvider = Provider.of<TextProvider>(context);

    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.only(top: 100.0),
          child: SizedBox(
            width: 300,
            child: Column(
              children: [
                Consumer<TextProvider>(
                    builder: (context, provider, child) => Text(
                          provider.text,
                          style: const TextStyle(
                              fontSize: 30, fontWeight: FontWeight.bold),
                        )),
                const SizedBox(
                  height: 20,
                ),
                TextField(
                  decoration: InputDecoration(
                      border: OutlineInputBorder(), labelText: 'Digite aqui'),
                  onChanged: (value) {
                    textProvider.updateText(value);
                  },
                ),
                const SizedBox(
                  height: 20,
                ),
                ElevatedButton(
                    onPressed: () {
                      Navigator.push(
                          context,
                          MaterialPageRoute(
                              builder: (context) => Secundaria()));
                    },
                    child: const Text('Trocar tela'))
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```