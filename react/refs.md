# React refs

When we want a component remember some information, but we don't want that information to trigger new renders, we can use ref.

````
import { useRef } from 'react';

const ref = useRef(0); // initial value
````

useRef returns a object like this 

````
{
  current: 0 // 0 is a inital value
}
````

You can access the current value of that ref through the ref.current property. 
This value is intentionally mutable, meaning you can both read and write to it.
It’s like a secret pocket of your component that React doesn’t track

When a piece of information is used for rendering, keep it in state.
When a piece of information is only needed by event handlers and changing it doesn’t require a re-render, using a ref may be more efficient.

#### Difference between state and ref

![Screenshot 2024-01-03 at 12.09.15.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_JWXrzH%2FScreenshot%202024-01-03%20at%2012.09.15.png)

#### How does useRef work inside?

Although both useState and useRef are provided by React, in principle useRef could be implemented on top of useState. You can imagine that inside of React, useRef is implemented like this:

````
function useRef(initialValue) {
  const [ref, unused] = useState({ current: initialValue });
  return ref;
}
````

During the first render, useRef returns { current: initialValue }. This object is stored by React, so during the next render the same object will be returned. Note how the state setter is unused in this example. It is unnecessary because useRef always needs to return the same object!

React provides a built-in version of useRef because it is common enough in practice. But you can think of it as a regular state variable without a setter. If you’re familiar with object-oriented programming, refs might remind you of instance fields—but instead of this.something you write somethingRef.current.

#### When to use refs

 - storing timeout ID's
 - storing and manipulating DOM elements (covering next)
 - storing other objects that aren't necessary to calculate the JSX

#### Best practices for refs

If we'll follow this principles, it will make our components more predictable

 - **Treat refs as an escape hatch.** Refs are useful when we work with external systems or browser API's. If much of our applications logic and data flow relies on refs, we might want to rethink your approach.
 - **Don't read or write ref.current during rendering.** If some information is needed during rendering, use state instead. Since React doesn’t know when ref.current changes, even reading it while rendering makes your component’s behavior difficult to predict. (The only exception to this is code like if (!ref.current) ref.current = new Thing() which only sets the ref once during the first render.)

Limitations of React state don’t apply to refs. For example, state acts like a snapshot for every render and doesn’t update synchronously. But when you mutate the current value of a ref, it changes immediately because the ref itself is a regular JavaScript object.

#### Refs and the DOM

The most common use case for a ref is to access a DOM element. 
For example, this is handy if you want to focus an input programmatically. 
When you pass a ref to a ref attribute in JSX, like <div ref={myRef}>, 
React will put the corresponding DOM element into myRef.current. 
Once the element is removed from the DOM, React will update myRef.current to be null.


#### How to manage a list of refs using a ref callback (deep dive)

Sometimes we might need a ref to each item in the list, and we don't know many we'll have.
One possible way is a single ref to elements parent element, another solution is
to **pass a function to the ref attribute** which was called ref callback.
React will call our ref callback with the DOM node when it's time to set the ref, and with null when it's time to clear it.
This let's us maintaion our own array or map, and access any ref by its index or some kind of ID.

````
import { useRef } from 'react';

export default function CatFriends() {
  const itemsRef = useRef(null);

  function scrollToId(itemId) {
    const map = getMap();
    const node = map.get(itemId);
    node.scrollIntoView({
      behavior: 'smooth',
      block: 'nearest',
      inline: 'center'
    });
  }

  function getMap() {
    if (!itemsRef.current) {
      // Initialize the Map on first usage.
      itemsRef.current = new Map();
    }
    return itemsRef.current;
  }

  return (
    <>
      <nav>
        <button onClick={() => scrollToId(0)}>
          Tom
        </button>
        <button onClick={() => scrollToId(5)}>
          Maru
        </button>
        <button onClick={() => scrollToId(9)}>
          Jellylorum
        </button>
      </nav>
      <div>
        <ul>
          {catList.map(cat => (
            <li
              key={cat.id}
              ref={(node) => {
                const map = getMap();
                if (node) {
                  map.set(cat.id, node);
                } else {
                  map.delete(cat.id);
                }
              }}
            >
              <img
                src={cat.imageUrl}
                alt={'Cat #' + cat.id}
              />
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}
````

#### Accessing another component’s DOM nodes

When we put a ref on a built-in component that outputs a vrowser element like `<input />`, React will set that ref's current property
to the corresponding DOM node (such as the actual <input /> in the browser).

But if we try to put a ref on own component such as `<MyInput />`, by default we'll get null.

Example:
````
function MyInput(props) {
  return <input {...props} />;
}

export default function MyForm() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
````

and also in the console react will print following warning

````
Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
````

By default React doesn't allow component access the DOM nodes of other components.
Not even for its own children! This is intentional. Refs are an escape hatch that should be used sparingly. Manually manipulating another component’s DOM nodes makes our code even more fragile.

Instead, components that want to expose their DOM nodes have to opt in to that behavior. A component can specify that it “forwards” its ref to one of its children. Here’s how MyInput can use the forwardRef API:

````
const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});
````

#### How it's work

1. <MyInput ref={inputRef} /> tells React to put the corresponding DOM node into inputRef.current. 
However, it’s up to the MyInput component to opt into that—by default, it doesn’t.
2. The MyInput component is declared using forwardRef. 
This opts it into receiving the inputRef from above as the second ref argument which is declared after props.
3. MyInput itself passes the ref it received to the <input> inside of it.


In design systems, it is a common pattern for low-level components like buttons, inputs, and so on, to forward their refs to their DOM nodes.
On the other hand, high-level components like forms, lists, or page sections usually won’t expose their DOM nodes to avoid accidental dependencies on the DOM structure.

#### Exposing a subset of the API with an imperative handle

In the above example, MyInput exposes the original DOM input element.
This lets the parent component call focus() on it. However, this also lets the parent component do something else - for example change its CSS styles.
In uncommon cases, you may want to restrict the exposed functionality. You can do that with useImperativeHandle:

````
import {
  forwardRef, 
  useRef, 
  useImperativeHandle
} from 'react';

const MyInput = forwardRef((props, ref) => {
  const realInputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    // Only expose focus and nothing else
    focus() {
      realInputRef.current.focus();
    },
  }));
  return <input {...props} ref={realInputRef} />;
});

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
````

Here, realInputRef inside MyInput holds the actual input DOM node. 
However, useImperativeHandle instructs React to provide your own special object as the value of a ref to the parent component. 
So inputRef.current inside the Form component will only have the focus method. 
In this case, the ref “handle” is not the DOM node, but the custom object you create inside useImperativeHandle call.

#### When React attaches the refs

In React, every update is split in two phases:

During render, React calls your components to figure out what should be on the screen.
During commit, React applies changes to the DOM.
In general, you don’t want to access refs during rendering. That goes for refs holding DOM nodes as well. During the first render, the DOM nodes have not yet been created, so ref.current will be null. 
And during the rendering of updates, the DOM nodes haven’t been updated yet. So it’s too early to read them.
React sets ref.current during the commit. Before updating the DOM, React sets the affected ref.current values to null. After updating the DOM, React immediately sets them to the corresponding DOM nodes.
Usually, you will access refs from event handlers. If you want to do something with a ref, but there is no particular event to do it in, you might need an Effect.

#### Flushing state updates synchronously with flushSync

Consider code like this, which adds a new todo and scrolls the screen down to the last child of the list. Notice how, for some reason, it always scrolls to the todo that was just before the last added one:
````
import { useState, useRef } from 'react';

export default function TodoList() {
  const listRef = useRef(null);
  const [text, setText] = useState('');
  const [todos, setTodos] = useState(
    initialTodos
  );

  function handleAdd() {
    const newTodo = { id: nextId++, text: text };
    setText('');
    setTodos([ ...todos, newTodo]);
    listRef.current.lastChild.scrollIntoView({
      behavior: 'smooth',
      block: 'nearest'
    });
  }

  return (
    <>
      <button onClick={handleAdd}>
        Add
      </button>
      <input
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <ul ref={listRef}>
        {todos.map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </>
  );
}

let nextId = 0;
let initialTodos = [];
for (let i = 0; i < 20; i++) {
  initialTodos.push({
    id: nextId++,
    text: 'Todo #' + (i + 1)
  });
}
````
The issue will be on these two lines:

````
setTodos([ ...todos, newTodo]);
listRef.current.lastChild.scrollIntoView();
````

In React, state updates are queued. Usually, this is what you want. However, here it causes a problem because setTodos does not immediately update the DOM. So the time you scroll the list to its last element, the todo has not yet been added. This is why scrolling always “lags behind” by one item.

To fix this issue, you can force React to update (“flush”) the DOM synchronously. To do this, import flushSync from react-dom and wrap the state update into a flushSync call:

````
flushSync(() => {
  setTodos([ ...todos, newTodo]);
});
listRef.current.lastChild.scrollIntoView();
````

This will instruct React to update the DOM synchronously right after the code wrapped in flushSync executes. As a result, the last todo will already be in the DOM by the time you try to scroll to it:

````
import { useState, useRef } from 'react';
import { flushSync } from 'react-dom';

export default function TodoList() {
  const listRef = useRef(null);
  const [text, setText] = useState('');
  const [todos, setTodos] = useState(
    initialTodos
  );

  function handleAdd() {
    const newTodo = { id: nextId++, text: text };
    flushSync(() => {
      setText('');
      setTodos([ ...todos, newTodo]);      
    });
    listRef.current.lastChild.scrollIntoView({
      behavior: 'smooth',
      block: 'nearest'
    });
  }

  return (
    <>
      <button onClick={handleAdd}>
        Add
      </button>
      <input
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <ul ref={listRef}>
        {todos.map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </>
  );
}

let nextId = 0;
let initialTodos = [];
for (let i = 0; i < 20; i++) {
  initialTodos.push({
    id: nextId++,
    text: 'Todo #' + (i + 1)
  });
}
````

#### Best practices for DOM manipulation with refs

Refs are an escape hatch. You should only use them when you have to “step outside React”. Common examples of this include managing focus, scroll position, or calling browser APIs that React does not expose.

If you stick to non-destructive actions like focusing and scrolling, you shouldn’t encounter any problems. However, if you try to modify the DOM manually, you can risk conflicting with the changes React is making.

**Avoid changing DOM nodes managed by React**. Modifying, adding children to, or removing children from elements that are managed by React can lead to inconsistent visual results or crashes like above.

However, this doesn’t mean that you can’t do it at all. It requires caution. **You can safely modify parts of the DOM that React has no reason to update**. For example, if some <div> is always empty in the JSX, React won’t have a reason to touch its children list. Therefore, it is safe to manually add or remove elements there.


Recap

Recap

Refs are a generic concept, but most often you’ll use them to hold DOM elements.
You instruct React to put a DOM node into myRef.current by passing <div ref={myRef}>.
Usually, you will use refs for non-destructive actions like focusing, scrolling, or measuring DOM elements.
A component doesn’t expose its DOM nodes by default. You can opt into exposing a DOM node by using forwardRef and passing the second ref argument down to a specific node.
Avoid changing DOM nodes managed by React.
If you do modify DOM nodes managed by React, modify parts that React has no reason to update.

-----------------------------------

Some rule of thumb :/ ?
**Whenever you need to track state in your React component which shouldn't trigger a re-render of your component, you can use React's useRef Hooks to create an instance variable for it.**


#### React callback ref

With callback ref, we don't have to use useEffect and useRef hooks anymore, because the callback ref gives us access to the DOM node on every render:

````
function ComponentWithRefRead() {
  const [text, setText] = React.useState('Some text ...');

  function handleOnChange(event) {
    setText(event.target.value);
  }

  const ref = (node) => {
    if (!node) return;

    const { width } = node.getBoundingClientRect();

    document.title = `Width:${width}`;
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleOnChange} />
      <div>
        <span ref={ref}>{text}</span>
      </div>
    </div>
  );
}
````
