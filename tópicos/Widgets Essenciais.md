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