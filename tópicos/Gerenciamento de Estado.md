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

### Estado Global
É um estado que precisa ser acessado em diferentes partes do aplicativo.
**Ex:** O tema do aplicativo

Existem várias formas de fazer este controle, as mais utilizadas são: 
- Provider
- Riverpod

Nos exemplos utilizaremos Provider.

---


