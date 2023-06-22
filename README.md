
## 🔨 Projeto: Client Control

O projeto do curso consiste em um gerenciamento de clientes de maneira que podemos cadastrar clientes, tipos de clientes e vincular os tipos cadastrados com os clientes utilizando abordagens de gerenciamento de estados.


## ✔️ Técnicas e tecnologias

**Veja mais de perto o que você aprenderá sobre** :
- `Provider`: Você aprenderá o que é o provider e o seu poder como gerenciador de estados.
- `Consumer`: Leia dados da única fonte da verdade através do Widget Consumer. 
- `Provider.of`: Entenda como acessar valores de estado fora da árvore de Widgets.
- `ChangeNotifier`: Possibilita preparar uma model para trabalhar como única fonte da verdade.
- `notifyListeners()`: Notifica as escutas de alterações no estado e notifica ao componente o novo estado.
- `MultiProvider`: É responsável por prover um meio de gerenciar multiplos providers na árvore de Widgets do projeto.


## 🛠️ Abrir e rodar o projeto

**Para executar este projeto você precisa:**

- Ter uma IDE, que pode ser o Android Studio ou Visual Studio Code, instalado na sua máquina.
- Ter a [SDK do Flutter](https://docs.flutter.dev/get-started/install) na versão 3.0.0 ou acima.

## 📚 Stateful, Inherited e Stateless

Quando gerenciamos estados, é importante que você saiba um ponto relevante sobre os widgets stateful,inherited e stateless.

Três ações - a alteração do estado, a atualização dos valores no componente e sua renderização em tela - estão extremamente ligadas ao tipo de widget que estamos utilizando. Para ter um estado e atualizá-lo, obrigatoriamente precisamos recorrer a um stateful widget ou inherited widget, ou seja, não podemos utilizar um stateless jamais, pois o stateless não tem estado.

Para saber mais sobre as diferenças de stateful e stateless widget, recomendamos a leitura do artigo Link https://www.alura.com.br/artigos/flutter-diferenca-entre-stateless-e-statefull-widget

## 📚 setState

Através da chamada do setState, o Flutter irá renderizar novamente a tela em questão que chamou o setState, de maneira que todas as alterações necessárias sejam exibidas visualmente com o estado mais atualizado.

## 📚  Gerenciamento de estado de aplicativo simples
Na documentação do Flutter eles recomendam o uso do provider para iniciarmos com gerencia de estados. 
https://docs.flutter.dev/data-and-backend/state-mgmt/simple

## 📚  Provider 

https://pub.dev/packages/provider

O provider é uma extensão tão robusta e aceita pela comunidade Flutter que há portabilidade do pacote para funcionar perfeitamente em Android, iOS, Linux, macOS, Web e Windows.


## Criando projeto 

Nesse projeto iremos ter a seguinte estrutura
lib 
- components 
- models
- pages
  
main.dart

Inicialmente iremos criar o arquivo components/hamburger_menu.dart

```python
import 'dart:io';
import 'package:flutter/material.dart';

class HamburgerMenu extends StatelessWidget {
  const HamburgerMenu({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        padding: EdgeInsets.zero,
        children: [
          const DrawerHeader(
            decoration: BoxDecoration(
              color: Colors.indigo,
            ),
            child: Text(
            'Menu',
            style: TextStyle(
              fontWeight: FontWeight.bold,
              color: Colors.white,
              fontSize: 20
            )),
          ),
          ListTile(
            title: const Text('Gerenciar clientes'),
            onTap: () {
              Navigator.pushNamed(context, '/');
            },
          ),
          const Divider(),
          ListTile(
            title: const Text('Tipos de clientes'),
            onTap: () {
              Navigator.pushNamed(context, '/tipos');
            },
          ),
          const Divider(),
          ListTile(
            title: const Text('Sair'),
            onTap: () {
              exit(0);
            },
          ),
        ],
      ),
    );
  }
}
```
Utilizamos o widget DrawerHeader para criamos nosso menu lateral, ja com nossa possiveis rotas.

No arquivo components/icon_picker.dar 

```python 

import 'package:flutter/material.dart';

Future<IconData?> showIconPicker(
  {required BuildContext context, IconData? defalutIcon}) async {

  final List<IconData> allIcons = [
    Icons.card_giftcard,
    Icons.card_membership,
    Icons.credit_card,
    Icons.credit_score,
    Icons.diamond,
    Icons.umbrella_sharp,
    Icons.favorite,
    Icons.headphones,
    Icons.home,
    Icons.car_repair,
    Icons.settings,
    Icons.flight,
    Icons.ac_unit,
    Icons.run_circle,
    Icons.book,
    Icons.sports_rugby_rounded,
    Icons.alarm,
    Icons.call,
    Icons.snowing,
    Icons.hearing,
    Icons.music_note,
    Icons.note,
    Icons.edit,
    Icons.sunny,
    Icons.radar,
  ];

  // selected icon
  // the selected icon is highlighed
  // so it looks different from the others
  IconData? selectedIcon = defalutIcon;

  await showDialog(
      context: context,
      builder: (_) => AlertDialog(
        title: const Text('Escolha um ícone'),
        content: Container(
          width: 320,
          height: 400,
          alignment: Alignment.center,
          // This grid view displays all selectable icons
          child: GridView.builder(
              gridDelegate: const SliverGridDelegateWithMaxCrossAxisExtent(
                  maxCrossAxisExtent: 60,
                  childAspectRatio: 1 / 1,
                  crossAxisSpacing: 10,
                  mainAxisSpacing: 10),
              itemCount: allIcons.length,
              itemBuilder: (_, index) => Container(
                key: ValueKey(allIcons[index].codePoint),
                padding: const EdgeInsets.all(10),
                child: Center(
                  child: IconButton(
                    // give the selected icon a different color
                    color: selectedIcon == allIcons[index]
                        ? Colors.indigo
                        : Colors.black,
                    iconSize: 30,
                    icon: Icon(
                      allIcons[index],
                    ),
                    onPressed: () {
                      selectedIcon = allIcons[index];
                      Navigator.of(context).pop();
                    },
                  ),
                ),
              )),
        ),
        actions: [
          ElevatedButton(
              onPressed: () {
                Navigator.of(context).pop();
              },
              child: const Text('Fechar'))
        ],
      ));

  return selectedIcon;
}
```

Dentro do GridView.builder iremos mostrar um conjunto de incos que podem ser escolhidos para cada tipo de cliente.

Agora iremos criar nossos models. O Nosso primeiro models é o models/client.dart 
```python
import 'package:client_control/models/client_type.dart';

class Client {
  String name;
  String email;
  ClientType type;

  Client({
    required this.name,
    required this.email,
    required this.type
  });
}

```
Depois iremos criar o models/client_type.dart 


```python
import 'package:flutter/material.dart';
class ClientType {
  String name;
  IconData? icon;

  ClientType({
    required this.name,
    required this.icon
  });
}
```

Agora iremos criar nossas pages. Inicialmente iremos criar nossa pages/clients_page.dart

```python
import 'package:client_control/models/client_type.dart';
import 'package:client_control/models/client.dart';
import 'package:flutter/material.dart';

import '../components/hamburger_menu.dart';

class ClientsPage extends StatefulWidget {
  const ClientsPage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  State<ClientsPage> createState() => _ClientsPageState();
}

class _ClientsPageState extends State<ClientsPage> {

  List<Client> clients = [
    Client(name: 'Geraldo', email: 'leo@email.com', type: ClientType(name: 'Platinum', icon: Icons.credit_card)),
    Client(name: 'Paulo', email: 'leo@email.com', type: ClientType(name: 'Golden', icon: Icons.card_membership)),
    Client(name: 'Caio', email: 'leo@email.com', type: ClientType(name: 'Titanium', icon: Icons.credit_score)),
    Client(name: 'Ruan', email: 'ruan@email.com', type: ClientType(name: 'Diamond', icon: Icons.diamond)),
  ];

  List<ClientType> types = [
    ClientType(name: 'Platinum', icon: Icons.credit_card),
    ClientType(name: 'Golden', icon: Icons.card_membership),
    ClientType(name: 'Titanium', icon: Icons.credit_score),
    ClientType(name: 'Diamond', icon: Icons.diamond),
  ];

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      drawer: const HamburgerMenu(),
      body: ListView.builder(
        itemCount: clients.length,
        itemBuilder: (context, index) {
          return Dismissible(
            key: UniqueKey(),
            background: Container(color: Colors.red),
            child: ListTile(
              leading: Icon(clients[index].type.icon),
              title: Text(clients[index].name + ' ('+ clients[index].type.name + ')'),
              iconColor: Colors.indigo,
            ),
            onDismissed: (direction) {
              setState(() {
                clients.removeAt(index);
              });
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.indigo,
        onPressed: (){createType(context);},
        tooltip: 'Add Tipo',
        child: const Icon(Icons.add),
      ),
    );
  }

  void createType(context) {
    TextEditingController nomeInput = TextEditingController();
    TextEditingController emailInput = TextEditingController();
    ClientType dropdownValue = types[0];

    showDialog(
        context: context,
        builder: (BuildContext context) {
            return AlertDialog(
                scrollable: true,
                title: const Text('Cadastrar cliente'),
                content: Padding(
                  padding: const EdgeInsets.all(8.0),
                  child: Form(
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: <Widget>[
                        TextFormField(
                          controller: nomeInput,
                          decoration: const InputDecoration(
                            labelText: 'Nome',
                            icon: Icon(Icons.account_box),
                          ),
                        ),
                        const Padding(padding: EdgeInsets.all(5)),
                        TextFormField(
                          controller: emailInput,
                          decoration: const InputDecoration(
                            labelText: 'Email',
                            icon: Icon(Icons.email),
                          ),
                        ),
                        const Padding(padding: EdgeInsets.all(5)),
                        StatefulBuilder(builder: (BuildContext context, StateSetter setState) {
                          return DropdownButton(
                            isExpanded: true,
                            value: dropdownValue,
                            icon: const Icon(Icons.arrow_downward),
                            style: const TextStyle(color: Colors.deepPurple),
                            underline: Container(
                              height: 2,
                              color: Colors.indigo,
                            ),
                            onChanged: (newValue) {
                              setState(() {
                                dropdownValue = newValue as ClientType;
                              });
                            },
                            items: types.map((ClientType type) {
                              return DropdownMenuItem<ClientType>(
                                value: type,
                                child: Text(type.name),
                              );
                            }).toList(),
                          );
                        }),
                      ],
                    ),
                  ),
                ),
                actions: [
                  TextButton(
                      child: const Text("Salvar"),
                      onPressed: () async {
                        setState(() {
                          clients.add(Client(name: nomeInput.text, email: emailInput.text, type: dropdownValue));
                        });
                        Navigator.pop(context);
                      }
                  ),
                  TextButton(
                    child: const Text("Cancelar"),
                    onPressed: () {
                      Navigator.pop(context);
                    }
                  ),
                ],
              );
            }
          );
  }
}

```
Nela ja podemos inserir clientes a nossa lista de clientes. Porém os dados eles são perdidos quando interagimos com outras páginas, mais iremos solucionar isso com o provider.

Agora iremos criar nossa segunda page ela vai se chamar pages/client_types_page.dart

```python
import 'package:client_control/models/client_type.dart';
import 'package:flutter/material.dart';

import '../components/hamburger_menu.dart';
import '../components/icon_picker.dart';

class ClientTypesPage extends StatefulWidget {
  const ClientTypesPage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  State<ClientTypesPage> createState() => _ClientTypesPageState();
}

class _ClientTypesPageState extends State<ClientTypesPage> {

  List<ClientType> types = [
    ClientType(name: 'Platinum', icon: Icons.credit_card),
    ClientType(name: 'Golden', icon: Icons.card_membership),
    ClientType(name: 'Titanium', icon: Icons.credit_score),
    ClientType(name: 'Diamond', icon: Icons.diamond),
  ];

  IconData? selectedIcon;

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      drawer: const HamburgerMenu(),
      body: ListView.builder(
        itemCount: types.length,
        itemBuilder: (context, index) {
          return Dismissible(
            key: UniqueKey(),
            background: Container(color: Colors.red),
            child: ListTile(
              leading: Icon(types[index].icon),
              title: Text(types[index].name),
              iconColor: Colors.deepOrange,
            ),
            onDismissed: (direction) {
              setState(() {
                types.removeAt(index);
              });
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.deepOrange,
        onPressed: (){createType(context);},
        tooltip: 'Add Tipo',
        child: const Icon(Icons.add),
      ),
    );
  }

  void createType(context) {
    TextEditingController nomeInput = TextEditingController();

    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          scrollable: true,
          title: const Text('Cadastrar tipo'),
          content: Padding(
            padding: const EdgeInsets.all(8.0),
            child: Form(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: <Widget>[
                  TextFormField(
                    controller: nomeInput,
                    decoration: const InputDecoration(
                      labelText: 'Nome',
                      icon: Icon(Icons.account_box),
                    ),
                  ),
                  const Padding(padding: EdgeInsets.all(5)),
                  StatefulBuilder(builder: (BuildContext context, StateSetter setState) {
                    return Column(children: [
                      const Padding(padding: EdgeInsets.all(5)),
                      selectedIcon != null ? Icon(selectedIcon, color: Colors.deepOrange) : const Text('Selecione um ícone'),
                      const Padding(padding: EdgeInsets.all(5)),
                      SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                            onPressed: () async {
                              final IconData? result = await showIconPicker(context: context, defalutIcon: selectedIcon);
                              setState(() {
                                selectedIcon = result;
                              });
                            },
                            child: const Text('Selecionar icone')
                        ),
                      ),
                    ]);
                  }),
                ],
              ),
            ),
          ),
          actions: [
            TextButton(
              child: const Text("Salvar"),
              onPressed: () {
                selectedIcon ??= Icons.credit_score;
                types.add(ClientType(name: nomeInput.text, icon: selectedIcon));
                selectedIcon = null;
                setState(() {});
                Navigator.pop(context);
              }
            ),
            TextButton(
              child: const Text("Cancelar"),
              onPressed: () async {
                selectedIcon = null;
                Navigator.pop(context);
              }
            )
          ],
        );
      }
    );
  }
}
```

Agora por ultimo iremos modificar nosso arquivo main.dart. 

```python
import 'pages/client_types_page.dart';
import 'package:flutter/material.dart';
import 'pages/clients_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Controle de clientes',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.indigo,
      ),
      initialRoute: '/',
      routes: {
        '/': (context) => const ClientsPage(title: 'Clientes'),
        '/tipos': (context) => const ClientTypesPage(title: 'Tipos de cliente'),
      },
    );
  }
}
```
Vamos agora testar e verificar tudo que esta acontecendo na nossa aplicação.

## 🛠️ Utilizando o Provider

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
