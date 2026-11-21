# tip-calculator
CARREL ALEXIE B. OBING
A simple Flutter application that calculates the tip amount and total bill based on the selected tip percentage (10%, 15%, or 20%).

import 'package:material_ui/material_ui.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Tip Calculator',
      theme: ThemeData(
        // This is the theme of your application.
        //
        // TRY THIS: Try running your application with "flutter run". You'll see
        // the application has a purple toolbar. Then, without quitting the app,
        // try changing the seedColor in the colorScheme below to Colors.green
        // and then invoke "hot reload" (save your changes or press the "hot
        // reload" button in a Flutter-supported IDE, or press "r" if you used
        // the command line to start the app).
        //
        // Notice that the counter didn't reset back to zero; the application
        // state is not lost during the reload. To reset the state, use hot
        // restart instead.
        //
        // This works for code too, not just values: Most code changes can be
        // tested with just a hot reload.
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Tip Calculator'),
    );
  }
}

// Parses the bill amount
double? parseBill(String input) {
  return double.tryParse(input.trim());
}

// Computes the tip and total bill
({double tip, double total}) computeTip(double bill, int percent) {
  double tip = bill * percent / 100;
  return (tip: tip, total: bill + tip);
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  // This widget is the home page of your application. It is stateful, meaning
  // that it has a State object (defined below) that contains values that affect
  // how it looks.

  // This class is the configuration for the state. It holds the values (in this
  // case the title) provided by the parent (in this case the App widget) and
  // used by the build method of the State. Fields in a Widget subclass are
  // always marked "final".

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  // Controller for bill amount
  final TextEditingController billController = TextEditingController();

  // Selected tip percentage starts at 15%
  int selectedPercent = 15;

  @override
  void dispose() {
    billController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Parse the bill amount
    double? bill = parseBill(billController.text);

    // Check if the bill input is valid
    bool isValid = bill != null && bill >= 0;

    // Stores the tip and total
    double tip = 0.0;
    double total = 0.0;

    if (isValid) {
      final result = computeTip(bill, selectedPercent);
      tip = result.tip;
      total = result.total;
    }

    // This method is rerun every time setState is called.
    //
    // The Flutter framework has been optimized to make rerunning build methods
    // fast, so that you can just rebuild anything that needs updating rather
    // than having to individually change instances of widgets.
    return Scaffold(
      appBar: AppBar(
        // TRY THIS: Try changing the app bar color to a specific color
        // and trigger a hot reload to see the AppBar change.
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,

        // Here we take the value from the MyHomePage object that was created by
        // the App.build method, and use it to set the appbar title.
        title: Text(widget.title),
      ),

      body: Padding(
        padding: const EdgeInsets.all(20.0),

        child: Column(
          // Column arranges the widgets vertically.
          // The main axis here is vertical.
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            // Bill amount input
            const Text(
              'Bill Amount',
              style: TextStyle(fontSize: 18),
            ),

            const SizedBox(height: 10),

            TextField(
              controller: billController,
              keyboardType: const TextInputType.numberWithOptions(
                decimal: true,
              ),
              decoration: const InputDecoration(
                prefixText: '₱ ',
                hintText: 'Enter amount',
                border: OutlineInputBorder(),
              ),
              onChanged: (value) {
                setState(() {
                  // Updates the tip and total when the bill changes
                });
              },
            ),

            const SizedBox(height: 20),

            // Tip percentage selection
            const Text(
              'Select Tip Percentage',
              style: TextStyle(fontSize: 18),
            ),

            const SizedBox(height: 10),

            SegmentedButton<int>(
              segments: const [
                ButtonSegment<int>(
                  value: 10,
                  label: Text('10%'),
                ),
                ButtonSegment<int>(
                  value: 15,
                  label: Text('15%'),
                ),
                ButtonSegment<int>(
                  value: 20,
                  label: Text('20%'),
                ),
              ],
              selected: {selectedPercent},
              onSelectionChanged: (selection) {
                setState(() {
                  // Changes the selected tip percentage
                  selectedPercent = selection.first;
                });
              },
            ),

            const SizedBox(height: 30),

            // Tip amount
            const Text(
              'Tip Amount',
              style: TextStyle(fontSize: 16),
            ),

            const SizedBox(height: 5),

            Text(
              isValid
                  ? '₱ ${tip.toStringAsFixed(2)}'
                  : 'Enter a valid amount',
              style: const TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 20),

            // Total bill
            const Text(
              'Total Bill',
              style: TextStyle(fontSize: 16),
            ),

            const SizedBox(height: 5),

            Text(
              isValid
                  ? '₱ ${total.toStringAsFixed(2)}'
                  : 'Enter a valid amount',
              style: const TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
                color: Colors.green,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
