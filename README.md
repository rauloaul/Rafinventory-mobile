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