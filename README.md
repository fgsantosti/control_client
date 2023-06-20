![Thumbnail GitHub](./thumb.png)

# Flutter: Gerenciamento de estados complexos

Esse curso de Flutter vai te ensinar a: 

-> O que é estado e gerenciadores de estados

-> Como instalar e utilizar o Provider como gerenciador de estados

-> Formular estados seguindo o conceito de single source of truth

-> Como organizar models que utilizam os conceitos do `change notifier`

-> Criar Widgets focados em estado e passagem de dados


## 🔨 Projeto: Client Control

O projeto do curso consiste em um gerenciamento de clientes de maneira que podemos cadastrar clientes, tipos de clientes e vincular os tipos cadastrados com os clientes utilizando abordagens de gerenciamento de estados.

![](./screenshot.png)

## ✔️ Técnicas e tecnologias

**Veja mais de perto o que você aprenderá sobre** :
- `Provider`: Você aprenderá o que é o provider e o seu poder como gerenciador de estados.
- `Consumer`: Leia dados da única fonte da verdade através do Widget Consumer. 
- `Provider.of`: Entenda como acessar valores de estado fora da árvore de Widgets.
- `ChangeNotifier`: Possibilita preparar uma model para trabalhar como única fonte da verdade.
- `notifyListeners()`: Notifica as escutas de alterações no estado e notifica ao componente o novo estado.
- `MultiProvider`: É responsável por prover um meio de gerenciar multiplos providers na árvore de Widgets do projeto.
- `Redux`: Entenda os conceitos e princípios dos gerenciadores com base no Redux.
- `BloC`: Veja como funciona a teoria dos gerenciadores que implementam o padrão BloC.

 


## 🛠️ Abrir e rodar o projeto

**Para executar este projeto você precisa:**

- Ter uma IDE, que pode ser o  [Android Studio](https://developer.android.com/) instalado na sua máquina
- Ter a [SDK do Flutter](https://docs.flutter.dev/get-started/install) na versão 3.0.0


## 📚 Mais informações do curso

Gostou do projeto e quer conhecer mais? Você pode [acessar o curso]() que desenvolve o projeto desde o começo!

Esse curso faz parte da [formação de Flutter da Alura](https://cursos.alura.com.br/formacao-flutter)

## stateful, inherited e stateless

Quando gerenciamos estados, é importante que você saiba um ponto relevante sobre os widgets stateful,inherited e stateless.

Três ações - a alteração do estado, a atualização dos valores no componente e sua renderização em tela - estão extremamente ligadas ao tipo de widget que estamos utilizando. Para ter um estado e atualizá-lo, obrigatoriamente precisamos recorrer a um stateful widget ou inherited widget, ou seja, não podemos utilizar um stateless jamais, pois o stateless não tem estado.

Para saber mais sobre as diferenças de stateful e stateless widget, recomendamos a leitura do artigo Link https://www.alura.com.br/artigos/flutter-diferenca-entre-stateless-e-statefull-widget

## setState

Através da chamada do setState, o Flutter irá renderizar novamente a tela em questão que chamou o setState, de maneira que todas as alterações necessárias sejam exibidas visualmente com o estado mais atualizado.

## Gerenciamento de estado de aplicativo simples

https://docs.flutter.dev/data-and-backend/state-mgmt/simple

## Provider 

https://pub.dev/packages/provider

O provider é uma extensão tão robusta e aceita pela comunidade Flutter que há portabilidade do pacote para funcionar perfeitamente em Android, iOS, Linux, macOS, Web e Windows.


## Utilizando o Provider

Iniciamente iremos instalar o pacote. 

```python
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  provider: ^6.0.5
```

Em models irei criar o arquivo clients.dart

```python 
import 'package:client_control/models/client.dart';
import 'package:flutter/material.dart';

class Clients extends ChangeNotifier {
  List<Client> clients;

  Clients({required this.clients});
}

```

No arquivo main.dart iremos fazer a seguinte mudança

```python 
void main() {
  runApp(ChangeNotifierProvider(
    create: (context) => Clients(clients: []), 
    child: const MyApp()
  ));
}
``` 

Todo widget filho do ChangeNotifierProvider vai ter acesso ao pai dele.

No nosso arquivo clients_page.dart iremos alterar a forma como estamos listando, inserindo e deletando os dados da nossa tela. 
```python 
      body: Consumer<Clients>(
          builder: (BuildContext context, Clients list, Widget? widget) {
        return ListView.builder(
          itemCount: list.clients.length,
          itemBuilder: (context, index) {
            return Dismissible(
              key: UniqueKey(),
              background: Container(color: Colors.red),
              child: ListTile(
                leading: Icon(list.clients[index].type.icon),
                title: Text(list.clients[index].name +
                    ' (' +
                    list.clients[index].type.name +
                    ')'),
                iconColor: Colors.indigo,
              ),
              onDismissed: (direction) {
                setState(() {
                  list.clients.removeAt(index);
                });
              },
            );
          },
        );
      }),

```

Outra parte que será alterada nesse ainda nesse arquivo é nas actions a funcao createType

```python
            actions: [
              Consumer<Clients>(builder:
                  (BuildContext context, Clients list, Widget? widget) {
                return TextButton(
                    child: const Text("Salvar"),
                    onPressed: () async {
                      setState(() {
                        list.add(Client(
                            name: nomeInput.text,
                            email: emailInput.text,
                            type: dropdownValue));
                      });
                      Navigator.pop(context);
                    });
              }),
              TextButton(
                  child: const Text("Calcelar"),
                  onPressed: () {
                    Navigator.pop(context);
                  }),
            ],
```

Agora iremos fazer uma classe para ficar ouvindo os typos de cliente, porem iremos ouvir as modificações fora da árvore de widegt. 

Vamos criar o arquivo models/types.dart e ficará dessa forma.

```python
import '../models/client_type.dart';
import 'package:flutter/material.dart';

class Types extends ChangeNotifier {
  List<ClientType> types;

  Types({required this.types});
}
```

Outra mudança que iremos fazer é no nosso arquivo pages/clients_pages.dart

Onde iremos comentar a lista contendo os diferentes tipos de cliente mocados.
Na função createTypes incrementaremos as linhas seguintes:

```python
  void createType(context) {
    TextEditingController nomeInput = TextEditingController();
    TextEditingController emailInput = TextEditingController();
    //pegar a lista de tipos do provider e colocar no dropdown
    //depois pegar o tipo selecionado e adicionar no cliente
    //fica ouvindo o estado do provider fora da arvore de widgets
    Types listtypes = Provider.of<Types>(context, listen: false);
    ClientType dropdownValue = listtypes.types[0];
    
    ...
```

Dessa forma poderemos agora também ouvir mudanças nos nossos tipos de cliente.