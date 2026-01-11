- **MVI architecture**
    - In MVI, the **View (widgets)** is only concerned with _what the UI should look like_ given a certain state.
    - The **Intent** represents user actions (like tapping a button).
    - The **Model (state)** is updated based on those actions.
      
- **Widgets don’t know** _**how**_ **the state changes**
    - Widgets are _decoupled_ from the business logic. They don’t contain the code that mutates state.
    - Instead, they just trigger an action (AsyncRedux) or method (Bloc/Cubit) and then rebuild when the state updates.
      
- **Widgets only know** _**what**_ **should change**
    - For example, a counter widget doesn’t care about the internal logic of incrementing a number.
    - It only knows: “When the state changes, display the new counter value.”

---
## Loading and error indicators

To show a loading indicator while an action is in progress, and to show an error message if it fails, you can use the `isWaiting` and `isFailed` methods provided by AsyncRedux:
![[Pasted image 20260107090518.png]]

