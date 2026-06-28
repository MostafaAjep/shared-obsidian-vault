# Variables 

`var` = "I'll figure it out"  
`final` = "decided once, forever"  
`const` = "decided at compile time, forever"  
`late` = "I promise I'll set it before you need it"

*80% of your local variables should be `final`. If you find yourself writing `var` a lot, that's a signal to pause and ask whether you actually need to reassign.

`var` and `final` are not the same _kind_ of keyword — they answer different questions:
- `var` / `String` / `List<Product>` → **what type is it?**
- `final` / `const` / `late` → **can I reassign it?**
### `final` locks the variable, not the value coming from the server

Every time your function runs, it creates a **brand new `final` variable**. The old one is gone. The new one gets locked with whatever the server returned this time.

```dart
Future<void> loadUser() async {
  final response = await dio.get('/user'); // new variable, locked to THIS response
  print(response.data['name']);
} 

// call it again
Future<void> loadUser() async {
  final response = await dio.get('/user'); // another new variable, locked to THIS response
  print(response.data['name']);
}
```

Think of it like this — every time the function runs, `final response` is a new box. You can put anything in the box. But once something is in the box, you can't swap it out **within that same function call**.

---
### Why this matters for async_redux specifically

This is exactly why your `reduce()` method returns a new `AppState` instead of just printing the value:

dart

```dart
class LoadUserAction extends ReduxAction<AppState> {
  @override
  Future<AppState?> reduce() async {
    final response = await dio.get('/user');         // temporary
    final name = response.data['name'] as String;   // temporary
    
    return state.copyWith(userName: name);           // handed to state — now remembered
  }
}
```

`response` and `name` get garbage collected the moment `reduce()` finishes. But `userName` inside `AppState` stays alive because the Redux store holds a reference to it — keeping it safe from the garbage collector for as long as the app is running.
### One line summary

Local variables are temporary workers — they do a job and leave. State is the company's memory — it holds onto the results of that work. The garbage collector only cleans up what nobody is holding a reference to anymore, and the state store keeps holding references to everything the UI needs.
### 'late'

In the following example, if the `temperature` variable is never used, then the expensive `readThermometer()` function is never called:

```
// This is the program's only call to readThermometer().
late String temperature = readThermometer(); // Lazily initialized.
```
  
### When is `dynamic` actually useful then?

there are a few real situations where it's the honest answer.

**1. JSON data before you've parsed it**

When Dio returns raw JSON, the values inside `response.data` are genuinely unknown to Dart until you parse them. The type really is dynamic at that point.

```dart
final response = await dio.get('/products');

// response.data is Map<String, dynamic> — the values could be
// a String, an int, a List, a nested Map — Dart has no way to know
final dynamic rawPrice = response.data['price'];
```

This is actually the most common place you'll see `dynamic` in Flutter — `Map<String, dynamic>` in every `fromJson`. The `dynamic` there is honest, not lazy.

**2. Working with a value that genuinely changes type**
Rare, but real:

```dart
dynamic result = someExternalFunction();
// sometimes returns a String, sometimes a List — out of your control
```

**3. Passing something through without caring about its type**


```dart
// async_redux middleware signature — the action could be anything
void Function(dynamic action) dispatch;
```

## Null Safety
With null safety, you must initialize the values of non-nullable variables before you use them:

```
int lineCount = 0;
```

You don't have to initialize a local variable where it's declared, but you do need to assign it a value before it's used. For example, the following code is valid because Dart can detect that `lineCount` is non-null by the time it's passed to `print()`:

```
int lineCount;

if (weLikeToCount) {
  lineCount = countLines();
} else {
  lineCount = 0;
}

print(lineCount);
```

---
### Why does it block `toString()` and `hashCode`?

This is the confusing part. You're probably thinking — "but `null` itself has `toString()`, so why block it?"

Because Dart's null safety works at the **type level**, not the **value level**. The compiler sees the _type_ `String?` and stops you — it doesn't check what the actual value is at runtime.

Think of it like airport security. They don't check if _you specifically_ are dangerous. They check _everyone_ because the rule is at the gate, not per person.


```dart
// null DOES have toString() — this works fine
print(null.toString()); // ✅ "null"

// but String? does NOT get that permission at compile time
String? name = null;
print(name.toString()); // ❌ Dart blocks it before it even runs
```
### How to fix it

**Option 1 — null check first**

```dart
String? name = getUserName();

if (name != null) {
  print(name.length);     // ✅ Dart now knows it's safe inside this block
  print(name.toString()); // ✅ fine
}
```

**Option 2 — null-aware operator `?.`**

```dart
String? name = getUserName();

print(name?.length);     // ✅ returns null if name is null, no crash
print(name?.toString()); // ✅ same
```

**Option 3 — force it with `!` (dangerous)**

```dart
String? name = getUserName();

print(name!.length); // ✅ compiles — but crashes at runtime if name is actually null
```

Only use `!` when you are absolutely certain the value cannot be null at that point. In practice, prefer the `if` check or `?.` instead.