# RAF-Inventory

Mobile inventory app.

## Assignment 7

### What are the main differences between stateless and stateful widget in Flutter?

- Stateless Widget:
    - Immutable and doesn't have internal state.
    - Ideal for static UI components that don't change.

- Stateful Widget:
    - Mutable with internal state.
    - Used for dynamic and interactive UI components that respond to user input and data changes.
    - May have slightly higher performance overhead.

### Explain all widgets that you used in this assignment.

- main.dart:

    - MaterialApp: This is the root widget of the application and represents the overall material design structure. It provides settings like the title of the app and theme information.

    - Scaffold: A scaffold is a basic skeletal structure for a Flutter app that provides an AppBar and a body for the content.

    - AppBar: It's a widget that displays an app bar at the top of the app, typically containing the app's title.

    - Column: A widget for arranging its children vertically.

    - Padding: Used to add padding around its child widgets.

    - GridView.count: Creates a grid layout with a fixed number of columns. It's used to display a grid of menu items.

    - Text: Widget for displaying text.

- menu.dart:

    - ShopItem: A class representing items in the shop menu, with properties like name, icon, and color.

    - MyHomePage: This widget represents the main content of the app, including the app bar and the grid of menu items.

    - ShopCard: A widget that displays each menu item as a card in the grid.

    - Material: A widget that provides a Material design card appearance with a specified color.

    - InkWell: It makes its child widget respond to user touches and provides the "onTap" callback for touch interactions.

    - ScaffoldMessenger: Used for displaying a SnackBar when a menu item is clicked.

    - Icon: Widget for displaying icons.

### Explain how you implemented the checklist above step-by-step

- First create flutter app in a new directory
    ```
    flutter create rinventory
    ```

- Create a new file named menu.dart in the rinventory/lib directory. Then import the following:
    ```
    import 'package:flutter/material.dart';
    ```

- Then from the main.dart file, move the code from line 39 to the end, to the `menu.dart` file, which includes the two classes below:
    - `class MyHomePage`
    - `class _MyHomePageState`

- In the `main.dart` add this following import:
    ```
    import 'package:rinventory/menu.dart';
    ```

- Then change into stateless widget with removing `MyHomePage(title: 'Flutter Demo Home Page')` so that it becomes `MyHomePage()`

- then in the `menu.dart` on class `MyHomePage extends StatelessWidget` change into this:
    ```dart
    class MyHomePage extends StatelessWidget {
        MyHomePage({Key? key}) : super(key: key);

        @override
        Widget build(BuildContext context) {
            return Scaffold(
                ...
            );
        }
    }
    ```

- To add text and cards, define the types of items that I want. Start by defining the type in the list:
    ```dart
    class ShopItem {
        final String name;
        final IconData icon;
        final Color color;

        ShopItem(this.name, this.icon, this.color);
    }
    ```

- Then Under the MyHomePage({Key? key}) : super(key: key); add items:
    ```dart
    final List<ShopItem> items = [
        ShopItem("View Items", Icons.checklist, Colors.red),
        ShopItem("Add Item", Icons.add_shopping_cart, Colors.green),
        ShopItem("Logout", Icons.logout, Colors.blue),
    ];
    ```

- Then add this widget build
    ```dart
    @override
    Widget build(BuildContext context) {
        return Scaffold(
            appBar: AppBar(
                title: const Text('RAF Inventory',
                ),
            ),
            body: SingleChildScrollView(
            
                child: Padding(
                    padding: const EdgeInsets.all(10.0), // Set padding for the page
                    child: Column(
                        // Widget to display children vertically
                        children: <Widget>[
                            const Padding(
                                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                                // Text widget to display text with center alignment and appropriate style
                                child: Text(
                                    'RAF Inventory', // Text indicating the shop name
                                    textAlign: TextAlign.center,
                                    style: TextStyle(
                                        fontSize: 30,
                                        fontWeight: FontWeight.bold,
                                    ),
                                ),
                            ),
                            // Grid layout
                            GridView.count(
                                // Container for our cards.
                                primary: true,
                                padding: const EdgeInsets.all(20),
                                crossAxisSpacing: 10,
                                mainAxisSpacing: 10,
                                crossAxisCount: 3,
                                shrinkWrap: true,
                                children: items.map((ShopItem item) {
                                // Iteration for each item
                                return ShopCard(item);
                                }).toList(),
                            ),
                        ],
                    ),
                ),
            ),
        );
    }
    ```

- Create new Stateless Widget to display the cards
    ```dart
    class ShopCard extends StatelessWidget {
        final ShopItem item;

        const ShopCard(this.item, {Key? key}) : super(key: key); // Constructor

        @override
        Widget build(BuildContext context) {
            return Material(
            color: item.color,
            child: InkWell(
                // Responsive touch area
                onTap: () {
                // Show a SnackBar when clicked
                ScaffoldMessenger.of(context)
                    ..hideCurrentSnackBar()
                    ..showSnackBar(SnackBar(
                        content: Text("You clicked the ${item.name} button!")));
                },
                child: Container(
                // Container to hold Icon and Text
                padding: const EdgeInsets.all(8),
                child: Center(
                    child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                        Icon(
                        item.icon,
                        color: Colors.white,
                        size: 30.0,
                        ),
                        const Padding(padding: EdgeInsets.all(3)),
                        Text(
                        item.name,
                        textAlign: TextAlign.center,
                        style: const TextStyle(color: Colors.white),
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

## Assignment 8

### Explain the difference between Navigator.push() and Navigator.pushReplacement(), accompanied by examples of the correct usage of both methods!

- `Navigator.push()` is used when we want to keep the current screen accessible, allowing users to return to it, while `Navigator.pushReplacement()` is used when we want to prevent users from going back to the previous screen, effectively replacing it with a new one.

### Explain each layout widget in Flutter and their respective usage contexts!

- **Drawer**: Is used as a navigation menu.
- **Form**: Is used to handle user form input and form validation.
- **TextFormField**: Is used for the user to input text.
- **AlertDialog**: Is used to notify the user.

### List the form input elements you used in this assignment and explain why you used these input elements!

- **TextFormField** enable users to input text, and we subsequently validate the entered text according to specified criteria.

### How is clean architecture implemented in a Flutter application?

- Implement Clean Architecture in a Flutter app by organizing code into layers: Entities, Use Cases, Repositories, and Frameworks/Drivers. Maintain a clear project structure and use dependency injection for loose coupling between layers.

### Explain how you implemented the checklist above step-by-step! (not just following the tutorial)

- Create Add Item Page

    ```dart
    import 'package:flutter/material.dart';
    import 'package:rinventory/widgets/left_drawer.dart';

    class ShopFormPage extends StatefulWidget {
    const ShopFormPage({super.key});

    @override
    State<ShopFormPage> createState() => _ShopFormPageState();
    }

    class _ShopFormPageState extends State<ShopFormPage> {
    final _formKey = GlobalKey<FormState>();
    String _name = "";
    int _amount = 0;
    String _description = "";
    String _category = "";
    int _power = 0;
    @override
    Widget build(BuildContext context) {
        return Scaffold(
        appBar: AppBar(
            title: const Center(
            child: Text(
                'Add Item Form',
            ),
            ),
            backgroundColor: Colors.indigo,
            foregroundColor: Colors.white,
        ),
        drawer: const LeftDrawer(),
        body: Form(
            key: _formKey,
            child: SingleChildScrollView(
            child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: TextFormField(
                    decoration: InputDecoration(
                        hintText: "Item Name",
                        labelText: "Item Name",
                        border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(5.0),
                        ),
                    ),
                    onChanged: (String? value) {
                        setState(() {
                        _name = value!;
                        });
                    },
                    validator: (String? value) {
                        if (value == null || value.isEmpty) {
                        return "Name cannot be empty!";
                        }
                        return null;
                    },
                    ),
                ),
                Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: TextFormField(
                    decoration: InputDecoration(
                        hintText: "Amount",
                        labelText: "Amount",
                        border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(5.0),
                        ),
                    ),
                    onChanged: (String? value) {
                        setState(() {
                        _amount = int.parse(value!);
                        });
                    },
                    validator: (String? value) {
                        if (value == null || value.isEmpty) {
                        return "Amount cannot be empty!";
                        }
                        if (int.tryParse(value) == null) {
                        return "Amount must be a number!";
                        }
                        return null;
                    },
                    ),
                ),
                Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: TextFormField(
                    decoration: InputDecoration(
                        hintText: "Description",
                        labelText: "Description",
                        border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(5.0),
                        ),
                    ),
                    onChanged: (String? value) {
                        setState(() {
                        _description = value!;
                        });
                    },
                    validator: (String? value) {
                        if (value == null || value.isEmpty) {
                        return "Description cannot be empty!";
                        }
                        return null;
                    },
                    ),
                ),
                Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: TextFormField(
                    decoration: InputDecoration(
                        hintText: "Category",
                        labelText: "Category",
                        border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(5.0),
                        ),
                    ),
                    onChanged: (String? value) {
                        setState(() {
                        _category = value!;
                        });
                    },
                    validator: (String? value) {
                        if (value == null || value.isEmpty) {
                        return "Category cannot be empty!";
                        }
                        return null;
                    },
                    ),
                ),
                Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: TextFormField(
                    decoration: InputDecoration(
                        hintText: "Power",
                        labelText: "Power",
                        border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(5.0),
                        ),
                    ),
                    onChanged: (String? value) {
                        setState(() {
                        _power = int.parse(value!);
                        });
                    },
                    validator: (String? value) {
                        if (value == null || value.isEmpty) {
                        return "Power cannot be empty!";
                        }
                        if (int.tryParse(value) == null) {
                        return "Power must be a number!";
                        }
                        return null;
                    },
                    ),
                ),
                Align(
                    alignment: Alignment.bottomCenter,
                    child: Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: ElevatedButton(
                        style: ButtonStyle(
                        backgroundColor:
                            MaterialStateProperty.all(Colors.indigo),
                        ),
                        onPressed: () {
                        if (_formKey.currentState!.validate()) {
                            showDialog(
                            context: context,
                            builder: (context) {
                                return AlertDialog(
                                title: const Text('Item successfully saved'),
                                content: SingleChildScrollView(
                                    child: Column(
                                    crossAxisAlignment:
                                        CrossAxisAlignment.start,
                                    children: [
                                        Text('Name: $_name'),
                                        Text('Amount: $_amount'),
                                        Text('Description: $_description'),
                                        Text('Category: $_category'),
                                        Text('Power: $_power'),
                                    ],
                                    ),
                                ),
                                actions: [
                                    TextButton(
                                    child: const Text('OK'),
                                    onPressed: () {
                                        Navigator.pop(context);
                                    },
                                    ),
                                ],
                                );
                            },
                            );
                        }
                        _formKey.currentState!.reset();
                        },
                        child: const Text(
                        "Save",
                        style: TextStyle(color: Colors.white),
                        ),
                    ),
                    ),
                ),
                ]
            )
            ),
        ),
        );
    }
    }
    ```

- Modify shop card, add this code:
    ```dart
    if (item.name == "Add Item") {
        Navigator.push(
            context,
            MaterialPageRoute(
            builder: (context) => const ShopFormPage(),
        ));
    }
    ```

- Add drawer:
    ```dart
    import 'package:flutter/material.dart';
    import 'package:rinventory/screens/menu.dart';
    import 'package:rinventory/screens/additem_form.dart';

    class LeftDrawer extends StatelessWidget {
    const LeftDrawer({super.key});

    @override
    Widget build(BuildContext context) {
        return Drawer(
        child: ListView(
            children: [
            const DrawerHeader(
                decoration: BoxDecoration(
                color: Colors.indigo,
                ),
                child: Column(
                children: [
                    Text(
                    'RAF Inventory',
                    textAlign: TextAlign.center,
                    style: TextStyle(
                        fontSize: 30,
                        fontWeight: FontWeight.bold,
                        color: Colors.white,
                    ),
                    ),
                    Padding(padding: EdgeInsets.all(10)),
                    Text("Welcome to RAF Inventory!",
                        textAlign: TextAlign.center,
                        style: TextStyle(fontSize: 15, color: Colors.white, fontWeight: FontWeight.normal,)
                        ),
                ],
                ),
            ),
            ListTile(
                leading: const Icon(Icons.home_outlined),
                title: const Text('Home Page'),
                
                onTap: () {
                Navigator.pushReplacement(
                    context,
                    MaterialPageRoute(
                        builder: (context) => MyHomePage(),
                    ));
                },
            ),
            ListTile(
                leading: const Icon(Icons.add_shopping_cart),
                title: const Text('Add Item'),
            
                onTap: () {
                Navigator.pushReplacement(
                    context,
                    MaterialPageRoute(
                        builder: (context) => const ShopFormPage(),
                    ));
                },
            ),
            ],
        ),
        );
    }
    }
    ```

- create new menu screen and delete the older one:
    ```dart
    import 'package:flutter/material.dart';
    import 'package:rinventory/widgets/left_drawer.dart';
    import 'package:rinventory/widgets/shop_card.dart';

    class MyHomePage extends StatelessWidget {
    MyHomePage({Key? key}) : super(key: key);
    
    final List<ShopItem> items = [
        ShopItem("View Items", Icons.checklist, Colors.red),
        ShopItem("Add Item", Icons.add_shopping_cart, Colors.green),
        ShopItem("Logout", Icons.logout, Colors.blue),
    ];

    @override
    Widget build(BuildContext context) {
        return Scaffold(
        appBar: AppBar(
            title: const Text(
            'RAF Inventory',
            ),
            backgroundColor: Colors.indigo,
            foregroundColor: Colors.white,
        ),
        drawer: const LeftDrawer(),
        body: SingleChildScrollView(
            // Scrolling wrapper widget
            child: Padding(
            padding: const EdgeInsets.all(10.0), // Set padding for the page
            child: Column(
                // Widget to display children vertically
                children: <Widget>[
                const Padding(
                    padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                    // Text widget to display text with center alignment and appropriate style
                    child: Text(
                    'RAF Inventory', // Text indicating the shop name
                    textAlign: TextAlign.center,
                    style: TextStyle(
                        fontSize: 30,
                        fontWeight: FontWeight.bold,
                    ),
                    ),
                ),
                // Grid layout
                GridView.count(
                    // Container for our cards.
                    primary: true,
                    padding: const EdgeInsets.all(20),
                    crossAxisSpacing: 10,
                    mainAxisSpacing: 10,
                    crossAxisCount: 3,
                    shrinkWrap: true,
                    children: items.map((ShopItem item) {
                    // Iteration for each item
                    return ShopCard(item);
                    }).toList(),
                ),
                ],
            ),
            ),
        ),
        );
    }
    }
    ```