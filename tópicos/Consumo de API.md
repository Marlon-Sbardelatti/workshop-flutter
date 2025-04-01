Vamos aprender na prática como consumir uma API e utilizar o pacote http.
Este será o nosso exemplo mais complexo.


> [!NOTE] http
> pacote que permite fazer e lidar com requisições http

# Exemplo prático

Neste exemplo vamos utilizar uma API pública para fazer os requests. Escolhi a API
https://rickandmortyapi.com/ pois ela tem uma boa quantidade de dados e também 
possui imagens de cada personagem.

Primeiramente devemos adicionar o pacote http ao nosso projeto

```shell
flutter pub add http
```

Vamos entender como funciona a API e a rota que vamos utilizar.

A rota GET da API tem como retorno o seguinte JSON: 

```json
{
  "info": {
    "count": 826,
    "pages": 42,
    "next": "https://rickandmortyapi.com/api/character/?page=2",
    "prev": null
  },
  "results": [
    {
      "id": 1,
      "name": "Rick Sanchez",
      "status": "Alive",
      "species": "Human",
      "type": "",
      "gender": "Male",
      "origin": {
        "name": "Earth",
        "url": "https://rickandmortyapi.com/api/location/1"
      },
      "location": {
        "name": "Earth",
        "url": "https://rickandmortyapi.com/api/location/20"
      },
      "image": "https://rickandmortyapi.com/api/character/avatar/1.jpeg",
      "episode": [
        "https://rickandmortyapi.com/api/episode/1",
        "https://rickandmortyapi.com/api/episode/2",
        // ...
      ],
      "url": "https://rickandmortyapi.com/api/character/1",
      "created": "2017-11-04T18:48:46.250Z"
    },
    // ...
  ]
}
```

Os personagens são retornados em formato JSON dentro do atributo results. Então 
primeiramente vamos criar uma classe que será o nosso "objeto" personagem. Nesta
classe vamos desserializar o JSON e transforma-lo em um objeto no flutter.

**character_model.dart**
```dart
class Character {
  final int id;
  final String name;
  final String? image;
  final String gender;
  final String species;
  final String originPlanet;

  Character({
    required this.id,
    required this.name,
    this.image,
    required this.gender,
    required this.species,
    required this.originPlanet,
  });

  factory Character.fromJson(Map<String, dynamic> json) {
    return Character(
        id: json['id'],
        name: json['name'],
        image: json['image'] ?? '',
        gender: json['gender'],
        species: json['species'],
        originPlanet: json['origin']['name'] ?? 'Unknown');
  }

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'image': image,
      'gender': gender,
      'species': species
    };
  }
}
```

Com essa classe pronta, agora podemos fazer um request para uma rota, transformar 
os resultados em objetos e depois renderizar todos no aplicativo.

No arquivo **main.dart** vamos fazer o import do pacote http

```dart
import 'package:http/http.dart' as http;
```

Agora vamos criar uma função que será responsável por fazer o request dos 
personagens. Em um cenário real onde seria necessário múltiplos requests
o correto seria criar uma classe controller e implementar funções de request
neste arquivo, como nosso exemplo é mais simples não vamos utilizar este padrão.

Também vamos adicionar essa função no nosso initState pois ela sempre será 
chamada quando nosso aplicativo for inicializado.

```dart
  Future<void> fetchCharacters() async {
    final res =
        await http.get(Uri.parse('https://rickandmortyapi.com/api/character/'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());
    } else {
      throw Exception('Erro ao carregar imagem');
    }
  }

  @override
  void initState() {
    super.initState();
    fetchCharacters();
  }
```

Percebemos que criamos uma variável local para armazenar os personagens, o que 
devemos fazer agora é criar um atributo nessa classe para armazenar esse resultado. 
Também vamos criar um atributo para fazer o controle de quando o request é realizado
com sucesso.

```dart
  List<Character> _characters = [];
  bool _isLoading = true;
```

Agora na nossa função de request vamos utilizar o setState para atualizar esses 
valores tendo em vista que estamos atualizando o estado do aplicativo.

```dart
  Future<void> fetchCharacters() async {
    final res =
        await http.get(Uri.parse('https://rickandmortyapi.com/api/character/'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());

      setState(() {
        _characters = characters;
        _isLoading = false;
      });
    } else {
      throw Exception('Erro ao carregar imagem');
    }
  }
```

na função build vamos verificar a variável \_isLoading enquanto ela for "true" vamos
renderizar uma animação de loading, quando \_isLoading passar a ser "false" 
renderizamos uma mensagem de sucesso (por enquanto).

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text('Rick and Morty'),
      ),
      body: _isLoading
          ? const Center(
              child: CircularProgressIndicator(),
            )
          : const Text('sucesso'),
    );
  }
```

Agora vamos renderizar os "results" da nossa request (substituindo o texto de sucesso)

```dart
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text('Rick and Morty'),
      ),
      body: _isLoading
          ? const Center(
              child: CircularProgressIndicator(),
            )
          : Padding(
              padding: const EdgeInsets.all(15),
              child: Column(
                children: [
                  Expanded(
                      child: GridView.builder(
                          gridDelegate:
                              const SliverGridDelegateWithFixedCrossAxisCount(
                                  crossAxisCount: 2),
                          itemCount: _characters.length,
                          itemBuilder: (context, index) {
                            return Text(index.toString());
                          }))
                ],
              ),
            ),
    );
  }
```

Para renderizar os elementos utilizamos um GridView chamando o método builder, 
essa função cria um grid de elementos, sendo que: 
- **itemCount:** o total de elementos;
- **gridDelegate:** o total de colunas no grid;
- **itemBuilder:** é uma função que retorna o widget que será construído;

No momento estamos apenas printando o index de cada elemento, percebemos que
temos apenas 20 elementos pois a API faz uso de um sistema de paginação, vamos
falar sobre paginação e implementar daqui a pouco. 

Agora podemos construir o widget que será nosso card para cada personagem, para
isso vamos adicionar o seguinte código 

```dart
itemBuilder: (context, index) {
final character = _characters[index];

return MouseRegion(
  cursor: SystemMouseCursors.click,
  child: GestureDetector(
	onTap: () {},
	child: Padding(
	  padding: const EdgeInsets.all(5.0),
	  child: Card(
		shape: RoundedRectangleBorder(
			borderRadius:
				BorderRadius.circular(15)),
		elevation: 5,
		child: Column(
		  crossAxisAlignment:
			  CrossAxisAlignment.stretch,
		  children: [
			Expanded(
				child: ClipRRect(
			  borderRadius:
				  const BorderRadius.vertical(
					  top: Radius.circular(15)),
			  child: Image.network(
				character.image ?? '',
				fit: BoxFit.cover,
			  ),
			)),
			Padding(
			  padding: const EdgeInsets.all(10),
			  child: Text(
				character.name,
				textAlign: TextAlign.center,
				style: TextStyle(
					fontWeight: FontWeight.bold,
					fontSize: 18),
			  ),
			),
		  ],
		),
	  ),
	),
  ),
);
}))
```

- **MouseRegion:** Utilizamos para dizer que aquele widget envolve interação com um mouse,  no nosso caso utilizamos para fazer a alteração do cursor quando o usuário coloca o mouse no widget;
- **GestureDetector:** Torna um widget "clicável";

Perceba que criamos um objeto `character` acessando através do index na lista
de characters

```dart 
final character = _characters[index];
```

Através desse objeto populamos o nosso widget com as informações daquele 
personagem

Agora vamos tornar cada card clicável e renderizar uma nova tela com mais detalhes
sobre cada personagem. Então primeiramente vamos criar uma nova classe para 
representar essa tela.

**character_details.dart**

```dart
class CharacterDetailsScreen extends StatefulWidget {
  final Character character;

  const CharacterDetailsScreen({super.key, required this.character});

  @override
  State<CharacterDetailsScreen> createState() => _CharacterDetailsScreenState();
}

class _CharacterDetailsScreenState extends State<CharacterDetailsScreen> {
  @override
  Widget build(BuildContext context) {
    return const Text('Teste');
  }
}
```


> [!NOTE] Atenção
> Repare que nesta tela recebemos um objeto Character como parâmetro e é através desse objeto que vamos criar a nossa tela

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Padding(
        padding: const EdgeInsets.all(38.0),
        child: Card(
          shape:
              RoundedRectangleBorder(borderRadius: BorderRadius.circular(15)),
          elevation: 5,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Padding(
                padding: const EdgeInsets.all(15.0),
                child: ClipRRect(
                  borderRadius: const BorderRadius.all(Radius.circular(15)),
                  child: Image.network(
                    widget.character.image ?? '',
                    fit: BoxFit.fitWidth,
                  ),
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(10),
                child: Text(
                  widget.character.name,
                  textAlign: TextAlign.center,
                  style: TextStyle(fontWeight: FontWeight.bold, fontSize: 30),
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(10),
                child: Row(
                  children: [
                    Text(
                      'Gender: ',
                      style:
                          TextStyle(fontWeight: FontWeight.bold, fontSize: 20),
                    ),
                    Text(
                      widget.character.gender,
                      style: TextStyle(fontSize: 20),
                    ),
                  ],
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(10),
                child: Row(
                  children: [
                    Text(
                      'Species: ',
                      style:
                          TextStyle(fontWeight: FontWeight.bold, fontSize: 20),
                    ),
                    Text(
                      widget.character.species,
                      style: TextStyle(fontSize: 20),
                    ),
                  ],
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(10),
                child: Row(
                  children: [
                    Text(
                      'Origin planet: ',
                      style:
                          TextStyle(fontWeight: FontWeight.bold, fontSize: 20),
                    ),
                    Text(
                      widget.character.originPlanet,
                      style: TextStyle(fontSize: 20),
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
```

Esta tela utiliza praticamente somente widgets que já conhecemos devemos nos atentar apenas para a forma em que utilizamos o nosso objeto character, utilizando
widget.atributo pois esse objeto é passado como um parâmetro no Construtor da tela.

Com essa tela criada precisamos  fazer o redirecionamento da tela principal para a tela
de detalhes de cada personagem. Para fazer isso vamos alterar o atributo onTap do
widget GestureDetector que é retornado no itemBuilder

```dart
  onTap: () {
	Navigator.push(
		context,
		MaterialPageRoute(
			builder: (context) =>
				CharacterDetailsScreen(
					character: character)));
  },
```

Com isso, agora podemos acessar os detalhes de cada personagem

### Tela principal

![[Pasted image 20250401152716.png]]

### Tela de detalhes

![[Pasted image 20250401152752.png]]

### Pagination

Vamos ver agora sobre paginação. Uma request idealmente nunca retorna todos
os resultados de uma vez, a request divide os resultados em páginas para melhorar
a performance do aplicativo. Na nossa API temos a seguinte estrutura: 

![[Pasted image 20250401154050.png]]

Temos os campos next e prev para acessar as páginas anteriores e seguintes.

#### Implementação pagination

Primeiramente vamos adicionar novos atributos na tela principal (**main.dart**)

```dart
  bool _isFetchingMore = false;
  String nextPageUrl = '';
```

- **\_isFetchingMore:** é utilizado para fazer o controle de novos dados;
- **nextPageUrl:** armazena a url da próxima página;

vamos adicionar uma verificação no setState da função fetchCharacters, nela 
verificamos se existe uma próxima página, caso exista armazenamos o valor
na variável nextPageUrl

```dart
  Future<void> fetchCharacters() async {
    final res =
        await http.get(Uri.parse('https://rickandmortyapi.com/api/character/'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());

      setState(() {
        if (data['info']['next'] != null) {
          nextPageUrl = data['info']['next'];
        } else {
          nextPageUrl = '';
        }
        _characters = characters;
        _isLoading = false;
      });
    } else {
      throw Exception('Erro ao carregar imagem');
    }
  }  Future<void> fetchCharacters() async {
    final res =
        await http.get(Uri.parse('https://rickandmortyapi.com/api/character/'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());

      setState(() {
        if (data['info']['next'] != null) {
          nextPageUrl = data['info']['next'];
        } else {
          nextPageUrl = '';
        }
        _characters = characters;
        _isLoading = false;
      });
    } else {
      throw Exception('Erro ao carregar imagem');
    }
  }
```

Agora vamos criar uma nova função que será utilizada para adicionar novos personagens a tela, ou seja, fazer uma requisição para outra página. 

```dart
  Future<void> _loadMoreCharacters() async {
    if (_isFetchingMore) {
      return;
    }

    setState(() {
      _isFetchingMore = true;
    });

    final res = await http.get(Uri.parse(nextPageUrl));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());

      setState(() {
        if (data['info']['next'] != null) {
          nextPageUrl = data['info']['next'];
        } else {
          nextPageUrl = '';
        }
        _characters += characters;
        _isLoading = false;
      });
    } else {
      throw Exception('Erro ao carregar imagem');
    }

    setState(() {
      _isFetchingMore = false;
    });
  }
```

Com essa função criada, devemos chama-lá sempre que o scroll do aplicativo chegar
ao fim. Para isso vamos criar outra função e outros atributos.

Criamos um atributo controller que é responsavel por verificar o estado do scroll 

```dart
  final ScrollController _scrollController = ScrollController();
```

Adicionamos o controller ao initState (a função \_onScroll ainda será criada)

```dart
  @override
  void initState() {
    super.initState();
    fetchCharacters();
    _scrollController.addListener(_onScroll);
  }
```


Adicionamos o controller no dispose dessa tela

```dart 
  @override
  void dispose() {
    _scrollController.dispose();
    super.dispose();
  }
```

Adicionamos o controller do scroll no atributo **controller** do `GridView.builder` 

```dart
  child: GridView.builder(
	  controller: _scrollController,
	  ...
  )
```


Agora vamos criar a função \_onScroll que anteriormente foi associada ao controller
do scroll no initState

```dart
  void _onScroll() async {
    if (_isFetchingMore || nextPageUrl == '') return;

    if (_scrollController.position.pixels >=
        _scrollController.position.maxScrollExtent - 100) {
      await _loadMoreCharacters();
    }
  }
```

Nesta função verificamos: 
- se algum request já foi feito ou se não existe uma próxima página, caso verdadeiro retornamos;
- se chegamos ao fim da tela chamamos a função que carrega a próxima página;

Com isso o sistema de paginação esta completo!

### Adicionando barra de pesquisa

Por fim vamos adicionar uma barra de pesquisa para fazer uma pesquisa por nome
de cada personagem

O filtro nesta API funciona através de parâmetros passados na url da request

![[Pasted image 20250401174637.png]]

Portanto vamos criar uma função que recebe uma String como parâmetro e vamos 
enviar esse valor para o endpoint da API

```dart
  Future<void> _getCharacterByParam(String param) async {
    if (param == '') {
      setState(() {
        _isLoading = true;
      });

      await fetchCharacters();
      return;
    }  Future<void> _getCharacterByParam(String param) async {
    if (param == '') {
      setState(() {
        _isLoading = true;
      });

      await fetchCharacters();
      return;
    }

    final res = await http.get(
        Uri.parse('https://rickandmortyapi.com/api/character/?name=$param'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());

      setState(() {
        if (data['info']['next'] != null) {
          nextPageUrl = data['info']['next'];
        } else {
          nextPageUrl = '';
        }
        _characters = characters;
      });
    } else {
      throw Exception('Erro ao carregar imagem');
    }
  }

    final res = await http.get(
        Uri.parse('https://rickandmortyapi.com/api/character/?name=$param'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data['results'];
      characters.addAll(
          charactersJson.map((json) => Character.fromJson(json)).toList());

      setState(() {
        if (data['info']['next'] != null) {
          nextPageUrl = data['info']['next'];
        } else {
          nextPageUrl = '';
        }
        _characters = characters;
      });
    } else {
      throw Exception('Erro ao carregar imagem');
    }
  }
```

Agora vamos adicionar um campo de texto que servirá como barra de pesquisa

```dart
	TextField(
		decoration: InputDecoration(
		  hintText: 'Search',
		  prefixIcon: const Icon(Icons.search),
		  border: OutlineInputBorder(
			borderRadius: BorderRadius.circular(10),
		  ),
		),
		onChanged: (value) {
	
		}),

```

Adicionamos um espaço entre o grid e a barra

```dart
  TextField(
	  decoration: InputDecoration(
		hintText: 'Search',
		prefixIcon: const Icon(Icons.search),
		border: OutlineInputBorder(
		  borderRadius: BorderRadius.circular(10),
		),
	  ),
	  onChanged: (value) async {
		if (_debounce?.isActive ?? false) _debounce!.cancel();

		_debounce =
			Timer(const Duration(milliseconds: 100), () async {
		  await _getCharacterByParam(value);
		});
	  }),
  const SizedBox(
	height: 10,
  ),

```

Toda vez que ocorrer um alteração no campo a função associada ao atributo 
onChanged será chamada

Para que essa função não seja chamada exatamente a todo momento e não ocorra
uma sobrecarga de requests, vamos criar um debounce.

> [!NOTE] debounce
> função que gera um intervalo antes que outra função seja chamada

criamos um atributo debounce

```dart
Timer? _debounce;
```

adicionamos o debounce no onChanged do nosso campo de texto

```dart
  onChanged: (value) async {
	if (_debounce?.isActive ?? false) _debounce!.cancel();

	_debounce =
		Timer(const Duration(milliseconds: 100), () async {
	  await _getCharacterByParam(value);
	});
  }),
```


### Cores

Podemos criar um arquivo para guardar cores personalizadas

**cores.dart**
```dart
import 'package:flutter/rendering.dart';

class MyColors {
  static const Color green = Color(0XFF97ce4c);
}
```

como exemplo podemos alterar a cor seed do nosso projeto 

**main.dart**

```dart
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: MyColors.green),
        useMaterial3: true,
      ),
      home: const MyHomePage(),
    );
  }
```

centralizando o titulo e trocando a cor

```dart
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(
          'Rick and Morty',
          style: TextStyle(color: Colors.white),
        ),
        centerTitle: true,
      ),
```