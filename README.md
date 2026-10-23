# Cairn examples

Example apps for **Cairn**, a research UI framework where apps are written in [Lean 4](https://lean-lang.org)
and every thing a person can do is a **claim**: a durable id (`^104`) with machine-checked
evidence of what it does. A thin Swift host runs the app natively on the Mac and Metal draws it.

▶ [TodoMVC, written in Lean, running natively on the Mac](https://cairn.iamnbutler.workers.dev/m/videos/5cf436dca2cd5034.mp4) (28 s)

These files are published to read. The framework isn't public yet, so they don't build on their
own. Cairn is early research.

## What's here

| File | What it is |
|---|---|
| [`lean/Examples/Basics.lean`](lean/Examples/Basics.lean) | A small todo list in one file: state, moves, the invariant, flows and the view. Start here. |
| [`lean/Examples/Basics/Proofs.lean`](lean/Examples/Basics/Proofs.lean) | Theorems about the view: presence (each item's controls exist exactly when the item does), the footer, and pressability. |
| [`lean/Examples/Basics/Tests.lean`](lean/Examples/Basics/Tests.lean) | Runtime checks on concrete states. |
| [`lean/Examples/Todo.lean`](lean/Examples/Todo.lean) | TodoMVC's claims: ^201 to ^224, mapped to the TodoMVC spec. |
| [`lean/Examples/Todo/`](lean/Examples/Todo/) | TodoMVC's model, view, tests and the ABI, with a [README](lean/Examples/Todo/README.md) that lists the design decisions and known gaps. |
| [`lean/Examples/Counter.lean`](lean/Examples/Counter.lean) | The first spike: a counter with Increment, and Reset only while there is something to reset. |
| [`host/Sources/CairnBasics/main.swift`](host/Sources/CairnBasics/main.swift) | The Swift side of Basics: it opens a window and hands Lean events. There is no app logic in it. |
| [`host/Sources/CairnCounter/main.swift`](host/Sources/CairnCounter/main.swift) | The same for Counter. |
| [`host/Sources/CairnTodo/main.swift`](host/Sources/CairnTodo/main.swift) | The same for TodoMVC. |

## What a claim looks like

```lean
move ^104 deleteItem (s : State) (k : Nat) "Delete an item" in ^106 where
  does "Press an item's delete button."
  sees "The item is gone, and its checkbox and delete button with it."
  enabled := has s k
  on press
    apply := delete k s
    post s' := s'.items = s.items.filter (·.id ≠ k)
    sound := by intro s k h; exact delete_items k s h
```

The kinds of claim:

- **move**: something a person does, like the move above.
- **flow**: a goal with a witness path from the start state.
- **invariant**: a property that holds in every reachable state.
- **assume**: something the proofs trust.
- **place**: a window or screen.

`update` is built only from the moves, so the app can't react to input any other way. Ids are
never reused: a claim that changes meaning gets a new id.

## License

MIT
