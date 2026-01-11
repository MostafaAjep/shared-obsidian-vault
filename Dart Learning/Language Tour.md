# Variables

When you mark a variable as `late` but initialize it at its declaration, then the initializer runs the first time the variable is used. This lazy initialization is handy in a couple of cases:

- The variable might not be needed, and initializing it is costly.
- You're initializing an instance variable, and its initializer needs access to `this`.

In the following example, if the `temperature` variable is never used, then the expensive `readThermometer()` function is never called:

```
// This is the program's only call to readThermometer().
late String temperature = readThermometer(); // Lazily initialized.
```
---
# Const
var foo = const [];
const baz = []; // Equivalent to `const []`
foo = [1, 2, 3]; // Was const []
you can change foo later to other value but you can not change baz 

--- 
A wildcard variable with the name `_` it means I don’t care about this value here its usage
- **Throwaway variable:** `_` is often used when a function requires a parameter, but you don’t need it. Example
```
  someFuture.then((_) => doSomething());
   ```
    Here, `_` receives the result of the future, but since you don’t use it, `_` acts as a placeholder.
- **Convention for unused values:** If you’re destructuring or looping and don’t care about certain values, `_` signals “ignore this.” Example:
    ```
    for (var _ in [1, 2, 3]) {
      count++;
    }
    ```
    The loop runs, but `_` isn’t used.
    ==Note== : Future Dart versions will break code that treats `_` as a real variable.
---
# operators

int a;
int b;
a = 0;
b = ++a;
 // Increment a before b gets its value.
assert(a == b); // 1 == 1
 a = 0; 
 b = a++; 
 // Increment a after b gets its value. 
 assert(a != b); // 1 != 0
 
---
# is Operator
####  Flutter-specific uses

- **Form validation**: Checking if a field’s controller is of a certain type before applying logic.
- **Navigation arguments**: When passing data between screens, you might receive a generic object and need to confirm its type before using it.
- **State management**: In providers or blocs, you might check if the current state is of a certain subclass before rendering widgets.
  `is` operator in Flutter apps is used to **safely check types before accessing properties or rendering UI**, especially when dealing with dynamic data or polymorphic models.

---
## Assignment operators

- Assign value to b if b is null; otherwise, b stays the same
  b ??= value;
- a += b   =>  a = a+b
 

---
##  Cascade notation
 Consider the following code:
```
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```
The constructor, `Paint()`, returns a `Paint` object. The code that follows the cascade notation operates on this object, ignoring any values that might be returned.

The previous example is equivalent to this code:
```
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;
```
Also it has null safety you can use it like that =>      ?..
 
 In Flutter, cascades are especially handy for:
- **Controllers** (`TextEditingController`, `AnimationController`, `ScrollController`)
- **Painters** (`Paint`)
- **Collections** (`List`, `Map`)
- Any object that needs multiple property assignments or method calls right after creation.
  Example:
```
class MyHomePage extends StatelessWidget {
  final controller = TextEditingController()
    ..text = 'Flutter'
    ..selection = TextSelection.collapsed(offset: 7);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: TextField(
          controller: controller,
        ),
      ),
    );
  }
}   
```
- When the `TextField` loads, it shows the word **Flutter**.
- The cursor is placed at the end of the text (position 7).
- Thanks to cascades, the setup is concise and readable.

---
# Bitwise operators

They shine in **low-level tasks** where performance and compact data representation matter.
Use when you need raw efficiency, compact storage, or are interfacing with binary data (e.g., Bluetooth packets, graphics, file formats, image processing).
 Working with Flags or Permissions : such as (Admin , Viewer , Editor) but only use when you have millions of users and want the app to be faster than if you use Enums.
#Senior_level

---

# Types

String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
-
You can use one or more underscores (`_`) as digit separators to make long number literals more readable. Multiple digit separators allow for higher level grouping. such as :
var n1 = 1_000_000;
-
o create a multi-line string, use a triple quote with either single or double quotation marks:

```
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

You can create a "raw" string by prefixing it with `r`:

```
var s = r'In a raw string, not even \n gets special treatment.';
```
-
### ✅ Why These Work
```
const aConstNum = 0;        // compile-time constant int
const aConstBool = true;    // compile-time constant bool
const aConstString = 'a constant string'; // compile-time constant string
const validConstString = '$aConstNum $aConstBool $aConstString';
```
- Each interpolated value is a **primitive constant** known at compile time.
- The compiler can fold them directly into the string literal → `"0 true a constant string"`.
### ❌ Why These Fail
```
var aNum = 0;               // runtime variable
var aBool = true;           // runtime variable
var aString = 'a string';   // runtime variable
const aConstList = [1, 2, 3]; // compile-time constant, but not a primitive
```
- `var` means the value is assigned at runtime, not compile time.
- `const aConstList` is a compile-time constant, but it’s a **list object**, not a primitive type. Dart doesn’t allow non-primitive constants inside const string interpolation.
-
# Symbol type
## ✅ When to Use Symbols

- **Reflection**: Looking up methods/fields dynamically (`dart:mirrors`).
- **Dynamic APIs**: When identifiers are passed around as strings (e.g., JSON configs, plugin systems).
- **Interop**: Some libraries use `Symbol` keys internally for metadata.
- **Minification** #Senior_level 
---
# Records
dksamda