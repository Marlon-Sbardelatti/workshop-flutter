## Criando um projeto Flutter

Para criar um projeto Flutter utilize o comando `flutter create nome-do-projeto`

``` bash
flutter create hello
```

## Estrutura de diretórios e pastas principais 

A estrutura `root` de um projeto Flutter será: 

![[principais-arquivos.png]]


### Principais diretórios e arquivos 

##### lib 
Podemos dizer que o diretório lib é o principal do projeto pois nele iremos encontrar:
* Arquivo `main.dart` que é o arquivo principal de todo projeto 
* Todos subdiretórios do nosso projeto. Ex: controllers, models, utils, etc.

![[lib.png]]
##### pubspec.yaml
Neste arquivo definimos as dependências do nosso projeto
![[dependencias.png]]


> [!NOTE] Adicionando uma dependência 
> **para a adicionarmos uma dependência (package) ao nosso projeto utilizamos o comando flutter pub + ação + nome do pacote**

```bash
flutter pub add shared_preferences
```

Alternativamente podemos primeiro adicionar o nome da dependência no nosso arquivo `pubspec.yaml`

```yaml
dependencies:
  shared_preferences: ^2.5.2
```

e executar o comando que baixará **todas** as dependências no arquivo: 

```bash
flutter pub get
```


##### Diretórios de sistema operacional
Diretórios com nome referente a algum sistema operacional possuem os arquivos de configuração específicos daquele SO.

**Ex:** No diretório android encontramos os seguintes arquivos: 
![[diretorio-android.png]]
Em resumo, cada arquivo serve para configurar algo referente a versão android 
(no diretório android) do seu aplicativo. Podemos dar ou retirar `permissões` ao aplicativo, configurar a versão do sdk utilizada, etc.


## Widgets - Stateless e Stateful

Como dito anteriormente, widgets são como `"blocos"` ou `"elementos"` que quando combinados formam o aplicativo


> [!NOTE] Widgets
> **componente visual para definir a interface de um aplicativo**

Um widget pode ser Stateful ou Stateless
#### Stateless
É um widget `sem controle de estado`, não é possível alterar dinamicamente o widget. São amplamente utilizados para criação de estruturas não mutáveis (telas,  imagens, etc), ou seja, tudo que não envolva entradas de dados dos usuários, acessos a APIs, etc.
#### Stateful
É um widget que `contém um controle de estado` e isso os torna mutáveis. É utilizado para tudo que necessita de uma entrada de dados. 
Um widget pode ser stateful quando necessita de: 
- entrada de dados;
- comportamentos ao iniciar a tela;
- formulários;
- acesso de uma informação por todos os componentes de uma tela;
- tudo que envolva uma requisição http `normalmente` será stateful

### ![[stateless-vs-stateful-widgets.png]]