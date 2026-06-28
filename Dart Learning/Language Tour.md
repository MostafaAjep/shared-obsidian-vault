# Variables

## Const
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
```
// Record type annotation in a variable declaration:
(String, int) record;
// Initialize it with a record expression:
record = ('A string', 123);
-
named go like this:
({int a, bool b}) record;
record = (a: 123, b: true);
```

```
        ({int a, int b}) recordAB = (a: 1, b: 2);
        ({int x, int y}) recordXY = (x: 3, y: 4);
        ```
        Here, `recordAB` has fields named `a` and `b`, while `recordXY` has fields named `x` and `y`. Even though both contain two integers, their _names_ differ — so Dart considers them **different types**. 👉 Assigning one to the other (`recordAB = recordXY`) causes a **compile error**.
```
        
```
Positional fields** are defined only by their order and types, not by names.

        (int a, int b) recordAB = (1, 2);
        (int x, int y) recordXY = (3, 4);
        recordAB = recordXY; // OK
        
        Here, the names `a` and `x` are just **documentation labels**. They don’t affect the type. Both records are simply `(int, int)` — so assignment works fine.
```
-
Use **positional records** when order alone is enough (like swapping two numbers). Use **named records** when meaning matters (like distinguishing `width` vs `height`).
-
Record fields are accessible through built-in getters. Records are immutable, so fields do not have setters.
-
Named fields expose getters of the same name. Positional fields expose getters of the name `$<position>`, skipping named fields:
```
var record = ('first', a: 2, b: true, 'last');

print(record.$1); // Prints 'first'
print(record.a); // Prints 2
print(record.b); // Prints true
print(record.$2); // Prints 'last'
```
-
There is no type declaration for individual record types. Records are structurally typed based on the types of their fields. A record's _shape_ (the set of its fields, the fields' types, and their names, if any) uniquely determines the type of a record.
-
Each field in a record has its own type. Field types can differ within the same record. The type system is aware of each field's type wherever it is accessed from the record:
```
(num, Object) pair = (42, 'a');

var first = pair.$1; // Static type `num`, runtime type `int`.
var second = pair.$2; // Static type `Object`, runtime type `String`.
```
-
### 🔹 Why Dart Added Records
- **Multiple returns:** Functions can return more than one value without ceremony.
- **Type safety:** Unlike `List` or `Map`, the compiler knows exactly what each field should be.
- **Flexibility:** You can mix positional and named fields depending on whether clarity or brevity matters.
- **Performance:** Records are optimized under the hood, so they’re lighter than defining a class just to hold two values.
-
### 🔹 When to Use Records vs. Old Way
- Use **classes** when the data has behavior (methods) or will be reused widely across your app.
- Use **records** when you just need to bundle a few values together temporarily — like returning multiple results from a function.
- Use **lists/maps** when the values are homogeneous or dynamic.
-
### 🔹 4. Async Operations with Different Types
Records are especially useful when combining multiple futures of different types.
**Example: Parallel API calls**

```
Future<(User, List<Post>)> fetchUserAndPosts() async {
  final user = await fetchUser();
  final posts = await fetchPosts();
  return (user, posts);
}

final (user, posts) = await fetchUserAndPosts();
```
✅ Cleaner than juggling two separate futures or wrapping them in a custom class.
-
```
final buttons = [
  (
    label: "Button I",
    icon: const Icon(Icons.upload_file),
    onPressed: () => print("Action -> Button I"),
  ),
  (
    label: "Button II",
    icon: const Icon(Icons.info),
    onPressed: () => print("Action -> Button II"),
  )
];
```

```
Column(
  children: buttons.map((btn) => ElevatedButton(
    onPressed: btn.onPressed,
    child: Row(
      children: [btn.icon, Text(btn.label)],
    ),
  )).toList(),
);
```
-
### Records and typedefs

You can choose to use [typedefs] to give the record type itself a name, and use that rather than writing out the full record type. This method allows you to state that some fields can be null (`?`), even if none of the current entries in the list have a null value.
String

```
typedef ButtonItem = ({String label, Icon icon, void Function()? onPressed});
final List<ButtonItem> buttons = [
  // ...
];
```
Code can work with the given button definitions the same way it would with simple class instances:
```
List<Container> widget = [
  for (var button in buttons)
    Container(
      margin: const EdgeInsets.all(4.0),
      child: OutlinedButton.icon(
        onPressed: button.onPressed,
        icon: button.icon,
        label: Text(button.label),
      ),
    ),
];
```
You could even decide to later change the record type to a class type to add methods.

// نموذج الـ response من الـ API
```
class UserResponse {
  final bool isActivated;
  final int remainingActivationDays;

  UserResponse({
    required this.isActivated,
    required this.remainingActivationDays,
  });
}

// دالة تحدد حالة المستخدم
String checkUserPlan(UserResponse response) {
  // نعمل record من المتغيرين المرتبطين
  var status = (response.isActivated, response.remainingActivationDays);

  // نعمل switch على record واحد بدل ifs كتير
  switch (status) {
    case (true, >0): // الحساب مفعل ولسه فيه أيام متبقية
      return "مسموح يدخل على الخطة المدفوعة ✅";
    case (true, 0): // مفعل لكن خلصت المدة
      return "انتهت مدة التفعيل ❌";
    case (false, _): // مش مفعل أصلاً
      return "الحساب غير مفعل ❌";
    default:
      return "حالة غير معروفة";
  }
}

void main() {
  var user1 = UserResponse(isActivated: true, remainingActivationDays: 5);
  var user2 = UserResponse(isActivated: true, remainingActivationDays: 0);
  var user3 = UserResponse(isActivated: false, remainingActivationDays: 10);

  print(checkUserPlan(user1)); // مسموح يدخل على الخطة المدفوعة ✅
  print(checkUserPlan(user2)); // انتهت مدة التفعيل ❌
  print(checkUserPlan(user3)); // الحساب غير مفعل ❌
}
```

---
# Collections
-
Iterables provide the `map()` method, which gives you all the results in a single object:
```
var teas = ['green', 'black', 'chamomile', 'earl grey'];
var loudTeas = teas.map((tea) => tea.toUpperCase());
loudTeas.forEach(print);
```
Note:
The object returned by `map()` is an Iterable that's _lazily evaluated_: your function isn't called until you ask for an item from the returned object.
-
