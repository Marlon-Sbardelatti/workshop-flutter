Quando queremos salvar pequenas informações no formato Chave-Valor (Key-Value)
utilizamos o pacote `shared_preferences`. Evite guardar informações sensíveis, por
exemplo: senha da conta, informações do cartão de crédito, etc.

## Shared Preferences 

> [!NOTE] Função
> Armazena dados localmente no dispositivo 

## Prática

Vamos continuar com o nosso exemplo de API, vamos adicionar um botão de favoritar
um personagem

**character_details.dart**
```dart
Column(
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
                        style: TextStyle(
                            fontWeight: FontWeight.bold, fontSize: 20),
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
                        style: TextStyle(
                            fontWeight: FontWeight.bold, fontSize: 20),
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
                        style: TextStyle(
                            fontWeight: FontWeight.bold, fontSize: 20),
                      ),
                      Text(
                        widget.character.originPlanet,
                        style: TextStyle(fontSize: 20),
                      ),
                    ],
                  ),
                ),
                Padding(
                  padding: const EdgeInsets.all(10),
                  child: IconButton(
                      onPressed: () {
                        print(widget.character.id);
                      },
                      icon: Icon(Icons.favorite)),
                )
              ],
            )
```

No momento estamos somente printando o id de cada personagem.

Agora vamos adicionar o shared preferences no nosso projeto

```shell
flutter pub add shared_preferences
```

Vamos criar um arquivo **shared_preferences_helper**

```dart
import 'package:shared_preferences/shared_preferences.dart';

class SharedPreferencesHelper {
  static Future<void> saveToFavorites(int id) async {
    final SharedPreferences prefs = await SharedPreferences.getInstance();
    String idString = id.toString();
    List<String>? listIds = prefs.getStringList('id_list');

    // já existe pelo menos um id
    if (listIds != null) {
      listIds.add(idString);
      await prefs.setStringList('id_list', listIds);
      return;
    } else {
      List<String> newListIds = [idString];
      await prefs.setStringList('id_list', newListIds);
      return;
    }
  }

  static Future<List<int>> getFavorites() async {
    final SharedPreferences prefs = await SharedPreferences.getInstance();
    List<String>? idStrings = prefs.getStringList('id_list');
    return idStrings?.map((id) => int.parse(id)).toList() ?? [];
  }
}
```

No arquivo **main.dart** vamos adicionar um botão no canto inferior direito. Este botão 
vai renderizar uma nova tela que fará o fetch dos personagens (com base nos ids salvos), no momento estamos somente printando os ids.

```dart
	  //appBar: ...
	  //body: ...
      floatingActionButton: FloatingActionButton(
        onPressed: () async {
          final res = await SharedPreferencesHelper.getFavorites();
          print(res);
        },
        child: Icon(Icons.favorite),
      ),
```

Agora vamos criar uma tela chamada **favorites_screen.dart** 

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class FavoritesScreen extends StatefulWidget {
  final List<int> ids;

  const FavoritesScreen({super.key, required this.ids});

  @override
  State<FavoritesScreen> createState() => _FavoritesScreenState();
}

class _FavoritesScreenState extends State<FavoritesScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: widget.ids.isEmpty
          ? Center(
              child: const Text('Você não favoritou nenhum personagem'),
            )
          : const Text('teste'),
    );
  }
}
```

no floating button na **main.dart** alteramos o código para: 
```dart
      floatingActionButton: FloatingActionButton(
        onPressed: () async {
          final ids = await SharedPreferencesHelper.getFavorites();
          Navigator.push(
              context,
              MaterialPageRoute(
                  builder: (context) => FavoritesScreen(ids: ids)));
        },
        child: Icon(Icons.favorite),
      ),
```

Agora voltando para **favorites_screen.dart**, vamos renderizar os personagens usando
uma nova função para fazer o fetch e um grid view

**função:** basicamente é a mesma função de antes só adicionamos os ids na url e removemos o if que servia para o pagination (não vamos implementar no momento)

```dart
  List<Character> _characters = [];
  bool _isLoading = true;

  Future<void> fetchCharacters() async {
    final res = await http.get(
        Uri.parse('https://rickandmortyapi.com/api/character/${widget.ids}'));

    if (res.statusCode == 200) {
      final data = jsonDecode(res.body);
      List<Character> characters = [];
      List<dynamic> charactersJson = data;
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

**initState:** Então chamamos essa função no initState caso a lista de ids não seja vazia

```dart
  @override
  void initState() {
    super.initState();
    if (widget.ids.isNotEmpty) {
      fetchCharacters();
    }
  }
```

**build:** Fazemos praticamente a mesma coisa que na tela principal só removemos widgets que serviam para a busca e para o pagination

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: widget.ids.isEmpty
          ? Center(
              child: const Text('Você não favoritou nenhum personagem'),
            )
          : _isLoading
              ? const Center(
                  child: CircularProgressIndicator(),
                )
              : Padding(
                  padding: const EdgeInsets.all(15),
                  child: Column(
                    children: [
                      Text(
                        'Favoritos',
                        style: TextStyle(
                            fontSize: 30, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(
                        height: 20,
                      ),
                      Expanded(
                        child: GridView.builder(
                          gridDelegate:
                              const SliverGridDelegateWithFixedCrossAxisCount(
                                  crossAxisCount: 2),
                          itemCount: _characters.length,
                          itemBuilder: (context, index) {
                            final character = _characters[index];

                            return MouseRegion(
                              cursor: SystemMouseCursors.click,
                              child: GestureDetector(
                                onTap: () {
                                  Navigator.push(
                                      context,
                                      MaterialPageRoute(
                                          builder: (context) =>
                                              CharacterDetailsScreen(
                                                  character: character)));
                                },
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
                          },
                        ),
                      ),
                    ],
                  ),
                ),
    );
  }
```


Para finalizar vamos adicionar um função para limpar os favoritos

**shared_preferences_helper.dart**
```dart
  static Future<void> removeIdList() async {
    final SharedPreferences prefs = await SharedPreferences.getInstance();
    await prefs.remove('id_list');
  }
```

**favorites_screen.dart**
```dart
	  //appBar: ...
	  //body: ...
      floatingActionButton: FloatingActionButton(
        onPressed: () async => await SharedPreferencesHelper.removeIdList(),
        child: Icon(Icons.delete),
      ),

```

Resultado final:

**Tela principal**
![[Pasted image 20250402091657.png]]

**character_details.dart**
![[Pasted image 20250402091758.png]]

**favorites_screen.dart**
![[Pasted image 20250402091833.png]]