In ReactJS, we use JSX, which stands for JavaScript XML, a standard introduced by ReactJS, similar to HTML. 

Browser only understands JavaScript, so we need to convert JSX to JS, using a react compiler like Babel. Todo that it takes the JSX and use React.createElement() function. Code could be written directly using React.createElement().

## React.createElement()

React.createElement() is the fundamental function used to create elements, called React Nodes. JSX is just a syntactic sugar for this function.

As an example:
```javascript
const jsx = <h1>Hello World!</h1>
```

is converted to:

```javascript
React.createElement("h1", null, "Hello, React!")
```

The syntax of the command is the following 

```javascript
React.createElement(type, props, ...children)
```

with :
- `type`: string, the HTML tag (\<h1>, etc.)
- `props`: className, id, onClick, etc.
- `...children`: content inside the element

Another example with children:

```html
<div>  
  <h1>Hello</h1>  
  <p>Welcome to React</p>  
</div>
```

is converted into:

```javascript
 const element = React.createElement(  
  "div",  
  null,  
  React.createElement("h1", null, "Hello"),  
  React.createElement("p", null, "Welcome to React")  
); 
```

For components like:

```javascript
const Jsx = <Card data = {cardData} />
```

we get:

```javascript
const element = React.createElement(Card, { data: cardData })
```

and when `<Card />` is used in other component, this `React.createElement` function is called with Card as the HTML tag.

The return of the function is a JS object looking like that:

``` javascript
{  
  type: "",  
  props: {  
      
  },  
  key: "",  
  ref: ""  
}
```

As an example:

```javascript
const element = React.createElement("h1", { className: "title" }, "Hello, React!");
```

turns into:

```javascript
{  
  type: "h1",  
  props: {  
    className: "title",  
    children: "Hello, React!"  
  },  
  key: null,  
  ref: null,  
  _owner: null,  
  _store: {}  
}
```

where `_owner`and `_store` are internal React properties.

Example with children:

``` javascript
const element = React.createElement(  
  "div",  
  { id: "container" },  
  React.createElement("h1", null, "Hello"),  
  React.createElement("p", null, "Welcome to React")  
);
```

turns into:

```
{  
  type: "div",  
  props: {  
    id: "container",  
    children: [  
      {  
        type: "h1",  
        props: { children: "Hello" },  
        key: null,  
        ref: null  
      },  
      {  
        type: "p",  
        props: { children: "Welcome to React" },  
        key: null,  
        ref: null  
      }  
    ]  
  },  
  key: null,  
  ref: null  
}
```

## Rendering

It refers to the process of taking React Elements and converting them into DOM elements.

They can be of two types:
- Initial Rendering
- Re-Rendering

### Initial Rendering

With this code:

```javascript
function App() {  
  return <h1>Hello, React!</h1>;  
}  
  
const root = ReactDOM.createRoot(document.getElementById("root"));  
root.render(<App />);
```

the following actions are done:

- `<App />` returns `<h1>Hello, React!</h1>`
- React converts into a React Element Object
- React creates a virtual DOM
- React updates the real DOM

DOM is actually constructed only one time because on large projects, we end up having massive nested JS objects.

The fact that the rendering of an app takes a lot of time only the first time is due to the use of the "Virtual DOM" and "reconciliation" algorithms.

### Re-renderding

It refers to the process of updating and executing again a component to reflect changes in the UI.

A component re-renders when:
- props change
- state changes
- parent re-renders

State is updated using `useState`.
"Props change" when a prop given to a React function changes (when the state of the var given as a prop changes for example).

Here is an example of a prop change when the `count` state is updated:

``` javascript
function Parent() {  
  const [count, setCount] = useState(0);  
  
  return (  
    <div>  
      <Child count={count} />  
      <button onClick={() => setCount(count + 1)}>Update Count</button>  
    </div>  
  );  
}  
  
  
function Child({ count }) {  
  console.log("Child Re-Rendered!");  
  
  return <h1>Count: {count}</h1>;  
}  
  
export default Parent;
``` 

Regarding parent re-render, if we want that a child never re-renders when its parent updates, then we can use `React.memo()`, the updates will occur only if one of the two other conditions is met.

## Strict Double Rendering

The use of `<React.StrictMode>` as it is done below will make all components re-render twice in development mode to detect side effects.

```javascript
ReactDOM.createRoot(document.getElementById("root")).render(  
  <React.StrictMode>  
    <App />  
  </React.StrictMode>  
);
```

## Virtual DOM

The V-DOM is a lightweight copy of the DOM maintained in Memory, taking the form of a JS object in a tree-like structure as seen above.

It is rendered during the initial render phase.

Then **Diffing** is done: on update, React creates a new V-DOM and compares it to the current one.

The comparison is used to update the Real DOM by finding the differences and only update the changed parts. This is called Reconciliation.

Diffing algorithms allows to update eficiently the DOM.


## Reconciliation

The steps of reconciliation are the ones mentioned above:
- create a new V-DOM
- compare it to the current one
- identify changes 
- update only what is necessary on the real DOM

There are some rules on this diffing algorithm:

1) Elements of different types cause full re-renders

Having something like the following:

```javascript
function App({ showText }) {  
  return showText ? <h1>Hello</h1> : <p>Hello</p>;  
}
```

will cause a full re-render because the HTML tag is completely changed.

2) Elements of the same type are updated efficiently:

With the following:
```javascript
function App({ text }) {  
  return <h1 className="title">{text}</h1>;  
}
```

will make React only update the text itself.


3) Lists are compared using keys

With the following, when not using keys:

```javascript 
function List({ items }) {  
  return (  
    <ul>  
      {items.map((item) => (  
        <li>{item}</li>  
      ))}  
    </ul>  
  );  
}
```

if a new item is added at the begging, re-renders all `<li>`, making things slow

Now with keys:

```javascript
function List({ items }) {  
  return (  
    <ul>  
      {items.map((item) => (  
        <li key={item.id}>{item.name}</li>  
      ))}  
    </ul>  
  );  
}
```

when adding a new element, only the necessary elements are updated.

## Performance Optimization Tools

1) As mentioned above, if a child receives no props from the parent, it is unncessary to update it when the parent updates, then we can wrap the child when exporting using `React.memo()` as such:

```javascript
export default React.memo(MyComponent)
```

2) if using heavy calculation on re-renders, `useMemo()` allows to benefit caching and prevents to make the calculation again when a function is called with the same arguments
 
```javascript
const computedValue = useMemo(() => expensiveComputation(number), [number]);
```

3) Hook `useCallback()`. When a component is re-rendered, so are the functions inside of it, with a new reference of that function. Using this hook, the same reference is used for next re-renders. In particular, when 