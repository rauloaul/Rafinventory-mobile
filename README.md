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

## Assignment 9

### Can we retrieve JSON data without creating a model first? If yes, is it better than creating a model before retrieving JSON data

- Retrieving JSON data from an API doesn't require creating a machine learning model. We can use tools like fetch in JavaScript or requests in Python for HTTP requests. Creating a model is necessary for tasks like prediction or classification based on patterns in data, not for simple data retrieval and processing.

### Explain the function of CookieRequest and explain why a CookieRequest instance needs to be shared with all components in a Flutter application.

- If there were a hypothetical CookieRequest class in Flutter, sharing an instance across components ensures consistent cookie handling throughout the application. This approach centralizes configuration, maintains consistency, and avoids redundancy in managing cookie-related settings for network requests.

### Explain the mechanism of fetching data from JSON until it can be displayed on Flutter.

1. HTTP Request:
    - Use a package like http to make an HTTP request to a JSON API.

2. Parse JSON:
    - Decode the JSON data using dart:convert.

3. Create Dart Objects:
    - Define Dart classes/models to represent the JSON structure.

4. Display in Widgets:
    - Use Flutter widgets to display data in the user interface.

### Explain the authentication mechanism from entering account data on Flutter to Django authentication completion and the display of menus on Flutter.

1. User Input in Flutter:
    - Users enter credentials in the Flutter app.

2. HTTP Request to Django:
    - Flutter sends credentials via HTTP to Django.

3. Django Authentication:
    - Django authenticates users.

4. Token Generation (Optional):
    - Django may generate a token for the authenticated user.

5. Response to Flutter:
    - Django responds with authentication status and, if applicable, a token.

6. Update Flutter UI:
    - Flutter updates the UI based on authentication status.

7. Display Menus in Flutter:
    - If authenticated, fetch and display menus using additional requests.

8. User Interaction in Flutter:
    - Users interact with menus triggering further actions or requests.

### List all the widgets you used in this assignment and explain their respective functions.

- **FutureBuilder** : to build a widget based on an async state.

- **ListView** : to display the children as a list layout.

- **GestureDetector** : to detect gesture on a widget

- **TextFormField** : to accept user text input

- **ElevatedButton** : to create a button

- **Container** : to contain a widget

### Explain how you implement the checklist above step by step!

- Create Login Screen
    ```dart
    // ignore_for_file: use_build_context_synchronously

    import 'package:rinventory/screens/menu.dart';
    import 'package:flutter/material.dart';
    import 'package:pbp_django_auth/pbp_django_auth.dart';
    import 'package:provider/provider.dart';

    void main() {
        runApp(const LoginApp());
    }

    class LoginApp extends StatelessWidget {
    const LoginApp({super.key});

    @override
    Widget build(BuildContext context) {
        return MaterialApp(
            title: 'Login',
            theme: ThemeData(
                primarySwatch: Colors.blue,
        ),
        home: const LoginPage(),
        );
        }
    }

    class LoginPage extends StatefulWidget {
        const LoginPage({super.key});

        @override
        _LoginPageState createState() => _LoginPageState();
    }

    class _LoginPageState extends State<LoginPage> {
        final TextEditingController _usernameController = TextEditingController();
        final TextEditingController _passwordController = TextEditingController();

        @override
        Widget build(BuildContext context) {
            final request = context.watch<CookieRequest>();
            return Scaffold(
                appBar: AppBar(
                    title: const Text('Login'),
                ),
                body: Container(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: [
                            TextField(
                                controller: _usernameController,
                                decoration: const InputDecoration(
                                    labelText: 'Username',
                                ),
                            ),
                            const SizedBox(height: 12.0),
                            TextField(
                                controller: _passwordController,
                                decoration: const InputDecoration(
                                    labelText: 'Password',
                                ),
                                obscureText: true,
                            ),
                            const SizedBox(height: 24.0),
                            ElevatedButton(
                                onPressed: () async {
                                    String username = _usernameController.text;
                                    String password = _passwordController.text;

                                    // Check credentials
                                    final response = await request.login("http://127.0.0.1:8000//auth/login/", {
                                    'username': username,
                                    'password': password,
                                    });

                                    if (request.loggedIn) {
                                        String message = response['message'];
                                        String uname = response['username'];
                                        Navigator.pushReplacement(
                                            context,
                                            MaterialPageRoute(builder: (context) => MyHomePage()),
                                        );
                                        ScaffoldMessenger.of(context)
                                            ..hideCurrentSnackBar()
                                            ..showSnackBar(
                                                SnackBar(content: Text("$message Welcome, $uname.")));
                                        } else {
                                        showDialog(
                                            context: context,
                                            builder: (context) => AlertDialog(
                                                title: const Text('Login Failed'),
                                                content:
                                                    Text(response['message']),
                                                actions: [
                                                    TextButton(
                                                        child: const Text('OK'),
                                                        onPressed: () {
                                                            Navigator.pop(context);
                                                        },
                                                    ),
                                                ],
                                            ),
                                        );
                                    }
                                },
                                child: const Text('Login'),
                            ),
                        ],
                    ),
                ),
            );
        }
    }
    ```

- Create custom model
    ```dart
    // To parse this JSON data, do
    //
    //     final product = productFromJson(jsonString);

    import 'dart:convert';

    List<Product> productFromJson(String str) => List<Product>.from(json.decode(str).map((x) => Product.fromJson(x)));

    String productToJson(List<Product> data) => json.encode(List<dynamic>.from(data.map((x) => x.toJson())));

    class Product {
        String model;
        int pk;
        Fields fields;

        Product({
            required this.model,
            required this.pk,
            required this.fields,
        });

        factory Product.fromJson(Map<String, dynamic> json) => Product(
            model: json["model"],
            pk: json["pk"],
            fields: Fields.fromJson(json["fields"]),
        );

        Map<String, dynamic> toJson() => {
            "model": model,
            "pk": pk,
            "fields": fields.toJson(),
        };
    }

    class Fields {
        int user;
        String name;
        int amount;
        String description;
        String category;
        int power;

        Fields({
            required this.user,
            required this.name,
            required this.amount,
            required this.description,
            required this.category,
            required this.power,
        });

        factory Fields.fromJson(Map<String, dynamic> json) => Fields(
            user: json["user"],
            name: json["name"],
            amount: json["amount"],
            description: json["description"],
            category: json["category"],
            power: json["power"],
        );

        Map<String, dynamic> toJson() => {
            "user": user,
            "name": name,
            "amount": amount,
            "description": description,
            "category": category,
            "power": power,
        };
    }
    ```

- Create View Produc Screen
    ```dart
    import 'package:flutter/material.dart';
    import 'package:http/http.dart' as http;
    import 'dart:convert';
    import 'package:rinventory/models/product.dart';
    import 'package:rinventory/screens/item_detail.dart';
    import 'package:rinventory/widgets/left_drawer.dart';

    class ProductPage extends StatefulWidget {
        const ProductPage({Key? key}) : super(key: key);

        @override
        _ProductPageState createState() => _ProductPageState();
    }

    class _ProductPageState extends State<ProductPage> {
    Future<List<Product>> fetchProduct() async {
        // TODO: Change the URL to your Django app's URL. Don't forget to add the trailing slash (/) if needed.
        var url = Uri.parse(
            'http:///127.0.0.1:8000/json/');
        var response = await http.get(
            url,
            headers: {"Content-Type": "application/json"},
        );

        // decode the response to JSON
        var data = jsonDecode(utf8.decode(response.bodyBytes));

        // convert the JSON to Product object
        List<Product> list_product = [];
        for (var d in data) {
            if (d != null) {
                list_product.add(Product.fromJson(d));
            }
        }
        return list_product;
    }

    @override
    Widget build(BuildContext context) {
        return Scaffold(
            appBar: AppBar(
            title: const Text('Product'),
            ),
            drawer: const LeftDrawer(),
            body: FutureBuilder(
                future: fetchProduct(),
                builder: (context, AsyncSnapshot snapshot) {
                    if (snapshot.data == null) {
                        return const Center(child: CircularProgressIndicator());
                    } else {
                        if (!snapshot.hasData) {
                        return const Column(
                            children: [
                            Text(
                                "No product data available.",
                                style:
                                    TextStyle(color: Color(0xff59A5D8), fontSize: 20),
                            ),
                            SizedBox(height: 8),
                            ],
                        );
                    } else {
                        return ListView.builder(
                        itemCount: snapshot.data!.length,
                        itemBuilder: (_, index) => InkWell(
                            onTap: () {
                                Navigator.push(
                                context,
                                MaterialPageRoute(
                                    builder: (context) =>
                                        ItemDetailPage(item: snapshot.data![index]),
                                ),
                                );
                            },
                            child: Container(
                                margin: const EdgeInsets.symmetric(
                                    horizontal: 16, vertical: 12),
                                padding: const EdgeInsets.all(20.0),
                                child: Column(
                                mainAxisAlignment: MainAxisAlignment.start,
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                    Text(
                                    "${snapshot.data![index].fields.name}",
                                    style: const TextStyle(
                                        fontSize: 18.0,
                                        fontWeight: FontWeight.bold,
                                    ),
                                    ),
                                    const SizedBox(height: 10),
                                    Text("${snapshot.data![index].fields.amount}"),
                                    const SizedBox(height: 10),
                                    Text(
                                        "${snapshot.data![index].fields.description}")
                                ],
                            ),
                            )
                        )
                        );
                    }
                    }
                }
                )
            );
        }
    }
    ```

- Create Detail Product Screen
    ```dart
    import 'package:flutter/material.dart';
    import 'package:rinventory/models/product.dart';
    // Import your item model

    class ItemDetailPage extends StatelessWidget {
    final Product item; // Pass the selected item to the page

    ItemDetailPage({required this.item});

    @override
    Widget build(BuildContext context) {
        return Scaffold(
        appBar: AppBar(
            title: const Text('Item Details'),
            leading: IconButton(
            icon: const Icon(Icons.arrow_back_sharp),
            tooltip: 'Go back to item list',
            onPressed: () {
                Navigator.pop(context);
            },
            ),
            backgroundColor: Colors.indigo,
            foregroundColor: Colors.white,
        ),
        body: Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
                Text(
                'Name: ${item.fields.name}',
                style: const TextStyle(fontSize: 24),
                ),
                const SizedBox(height: 5),
                Text(
                'Amount: ${item.fields.amount}',
                style: const TextStyle(fontSize: 16),
                ),
                const SizedBox(height: 5),
                Text(
                'Description: ${item.fields.description}',
                style: const TextStyle(fontSize: 16),
                ),
                const SizedBox(height: 5),
                Text(
                'Category: ${item.fields.category}',
                style: const TextStyle(fontSize: 16),
                ),
                const SizedBox(height: 5),
                Text(
                'Power: ${item.fields.power}',
                style: const TextStyle(fontSize: 16),
                ),
                // Add more details as needed
            ],
            ),
        ),
        );
    }
    }
    ```