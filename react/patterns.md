# React Patterns

### Container/Presentational Pattern

In React, one way to enforce separation of concerns is by using the Container/Presentational pattern. With this pattern, we can separate the view from the application logic.

Example:

We want to fetch 6 dog images and renders these images on the screen

````
// file
DodImages.js

export default function DogImages({ dogs }) {
  return dogs.map((dog, i) => <img src={dog} key={i} alt="Dog" />);
}

// file
DogImagesContainer.js

export default class DogImagesContainer extends React.Component {
  constructor() {
    super();
    this.state = {
      dogs: []
    };
  }

  componentDidMount() {
    fetch("https://dog.ceo/api/breed/labrador/images/random/6")
      .then(res => res.json())
      .then(({ message }) => this.setState({ dogs: message }));
  }

  render() {
    return <DogImages dogs={this.state.dogs} />;
  }
}
````

**Presentational Components:** Components that care about how data is shown to the user. In this example, that’s the rendering the list of dog images.

**Container Components:** Components that care about what data is shown to the user. In this example, that’s fetching the dog images.

By separating these two concerns, we can make our code easier to reason about and maintain.

<br/>
<br/>

A **presentational component** receives its data through props and its'
primary function is to simply display the data it receives the way we want them to,
including styles, without modifying the data.Presentational components are usually stateless: they do not contain their own React state, unless they need a state for UI purposes. The data they receive, is not altered by the presentational components themselves.
Presentational components receive their data from container components.

<br/>
<br/>

The primary function of **container components** is to pass data to presentational components, which they contain. Container components themselves usually don’t render any other components besides the presentational components that care about their data. Since they don’t render anything themselves, they usually do not contain any styling either.


Combining these two components together makes it possible to separate handling application logic with the view
[//]: # (### Compound Components Pattern)

#### Hooks

In many cases, the Container/Presentational pattern can be replaced with React Hooks.

````
export default function useDogImages() {
  const [dogs, setDogs] = useState([]);

  useEffect(() => {
    fetch("https://dog.ceo/api/breed/labrador/images/random/6")
      .then((res) => res.json())
      .then(({ message }) => setDogs(message));
  }, []);

  return dogs;
}
````
By using this hook, we no longer need the wrapping DogImagesContainer container component to fetch the data, and send this to the presentational DogImages component. Instead, we can use this hook directly in our presentational DogImages component!
````
export default function DogImages() {
  const dogs = useDogImages();

  return dogs.map((dog, i) => <img src={dog} key={i} alt="Dog" />);
}
````

By using the useDogImages hook, we still separated the application logic from the view. We’re simply using the returned data from the useDogImages hook, without modifying that data within the DogImages component.

Hooks make it easy to separate logic and view in a component, just like the Container/Presentational pattern.


### Pros

There are many benefits to using the Container/Presentational pattern.

The Container/Presentational pattern encourages the separation of concerns. Presentational components can be pure functions which are responsible for the UI, whereas container components are responsible for the state and data of the application. This makes it easy to enforce the separation of concerns.

Presentational components are easily made reusable, as they simply display data without altering this data. We can reuse the presentational components throughout our application for different purposes.

Since presentational components don’t alter the application logic, the appearance of presentational components can easily be altered by someone without knowledge of the codebase, for example a designer. If the presentational component was reused in many parts of the application, the change can be consistent throughout the app.

Testing presentational components is easy, as they are usually pure functions. We know what the components will render based on which data we pass, without having to mock a data store.

### Cons

The Container/Presentational pattern makes it easy to separate application logic from rendering logic. However, Hooks make it possible to achieve the same result without having to use the Container/Presentational pattern, and without having to rewrite a stateless functional component into a class component.Note that today, we don’t need to create class components to use state anymore.

Although we can still use the Container/Presentational pattern, even with React Hooks, this pattern can easily be an overkill in smaller sized application.

### Higher-Order Components (HOC pattern)

One way of being able to reuse the same logic in multiple components, is by using the higher order component pattern. This pattern allows us to reuse component logic throughout our application.

Example:

Say that we always wanted to add a certain styling to multiple components in our application. Instead of creating a style object locally each time, we can simply create a HOC that adds the style objects to the component that we pass to it
````
function withStyles(Component) {
    return props => {
        const style = { padding: '0.2rem', margin: '1rem' }
        return <Component style={style} {...props} />
    }
}

const Button = () = <button>Click me!</button>
const Text = () => <p>Hello World!</p>

const StyledButton = withStyles(Button)
const StyledText = withStyles(Text)
````

We just created a StyledButton and StyledText component, which are the modified versions of the Button and Text component. 
They now both contain the style that got added in the withStyles HOC!

Let's create a HOC called withLoader, which should receive the element which should display Loading... until data is fetched.


````
function withLoader(Element, url) {
  return (props) => {};
}
````


````
function withLoader(Element, url) {
  return (props) => {
    const [data, setData] = useState(null);

    useEffect(() => {
      async function getData() {
        const res = await fetch(url);
        const data = await res.json();
        setData(data);
      }

      getData();
    }, []);

    if (!data) {
      return <div>Loading...</div>;
    }

    return <Element {...props} data={data} />;
};
````

1. In the useEffect hook, the withLoader HOC fetches the data from the API endpoint that we pass as the value of url. While the data hasn’t returned yet, we return the element containing the Loading... text.
2. Once the data has been fetched, we set data equal to the data that has been fetched. Since data is no longer null, we can display the element that we passed to the HOC!

Also here is the usage

````
export default withLoader(
  SomeComponent,
  "https://dog.ceo/api/breed/labrador/images/random/6"
);
````

The HOC pattern allows us to provide the same logic to multiple components, while keeping all the logic in one single place. 

#### Composing

We can also compose multiple HOCs together. Let's say that we want to add functionality that shows a Hovering! text box when user hovers over the DogImages list.
We need to create a HOC that provides a hovering prop to the element that we pass. Based on that prop, we can conditionally render the text box based on whether the user is hovering over the DogImages list.
````
export default function withHover(Element) {
  return props => {
    const [hovering, setHover] = useState(false);

    return (
      <Element
        {...props}
        hovering={hovering}
        onMouseEnter={() => setHover(true)}
        onMouseLeave={() => setHover(false)}
      />
    );
  };
````
  
  
and here is the usage
````
export default withHover(
  withLoader(DogImages, "https://dog.ceo/api/breed/labrador/images/random/6")
);  
````

````
function DogImages(props) {
  return (
    <div {...props}>
      {props.hovering && <div id="hover">Hovering!</div>}
      <div id="list">
        {props.data.message.map((dog, index) => (
          <img src={dog} alt="Dog" key={index} />
        ))}
      </div>
    </div>
  );
}

export default withHover(
  withLoader(DogImages, "https://dog.ceo/api/breed/labrador/images/random/6")
);
````

#### Hooks

In some cases we can replace the HOC pattern with hooks. Let's replace withHover HOC with useHover hook.

````
export default function useHover() {
  const [hovering, setHover] = useState(false);
  const ref = useRef(null);

  const handleMouseOver = () => setHover(true);
  const handleMouseOut = () => setHover(false);
  
    useEffect(() => {
      const node = ref.current;
            
      if (node) {
        node.addEventListener('mouseover', handleMouseOver);
        node.addEventListener('mouseout', handleMouseOut);
        
        return () => {
          node.removeEventListener('mouseover', handleMouseOver);
          node.removeEventListener('mouseout', handleMouseOut);
        }
      }
    
    }, [ref.current]);
    
    return [ref, hovering]
}
````

and here is the usage
````
function DogImages(props) {
  const [hoverRef, hovering] = useHover();

  return (
    <div ref={hoverRef} {...props}>
      {hovering && <div id="hover">Hovering!</div>}
      <div id="list">
        {props.data.message.map((dog, index) => (
          <img src={dog} alt="Dog" key={index} />
        ))}
      </div>
    </div>
  );
}

export default withLoader(
  DogImages,
  "https://dog.ceo/api/breed/labrador/images/random/6"
);
````

So instead of wrapping the DogImages component with the withHover component, we can simply use the useHover hook within the component directly.

But we need to understand that hooks are not a replacement for HOCs.

Hooks are a more direct way to reuse stateful logic, while HOCs are a more indirect way to reuse component logic. Hooks are more flexible, but HOCs are more powerful.

"In most cases, Hooks will be sufficient and can help reduce nesting in your tree" - React Docs

As the React docs tell us, using Hooks can reduce the depth of the component tree. Using the HOC pattern, it’s easy to end up with a deeply nested component tree.

Using Higer Order Components makes it possible to provide the same logic to many components, 
while keeping that logic all in one single place.Hooks allow us to add custom behavior from within the component, which could potentially increase the risk of introducing bugs compared to the HOC pattern if multiple components rely on this behavior.


#### Best use-cases for a HOC:

 - The same, uncustomized behavior needs to be used by many components throughout the application.
 - The component can work standalone, without the added custom logic.

#### Best use-cases for Hooks:

 - The behavior has to be customized for each component that uses it. 
 - The behavior adds many properties to the component.
 - The behavior is not spread throughout the application, only one or a few components use the behavior.

### Pros
Using the Higher Order Component pattern allows us to keep logic that we want to re-use all in one place. 
This reduces the risk of accidentally spreading bugs throughout the application by duplicationg code over and over, potentially introducing new bugs each ttime.
By keeping the logic all in one place, we can keep our code DRY and easily enforce separation od concerns.

### Cons
The name of the prop that a HOC can pass to an element, can cause a naming collision.
In this case, the withStyles HOC adds a prop called style to the element that we pass to it. However, the Button component already had a prop called style, which will be overwritten! Make sure that the HOC can handle accidental name collision, by either renaming the prop or merging the props.

### Render Props Pattern

Another way of making components very reusable, is by using the render prop pattern.
A render prop is a prop on a component, which value is a function that returns a JSX element. 
The component itself does not render anything besides the render prop. Instead, the component simply calls the render prop, instead of implementing its own rendering logic.

Imagine that we have a Title component. In this case, the Title component shouldn’t do anything besides rendering the value that we pass.

Simple example:

````
<Title render={() => <h1>I am a render prop!</h1>} />
````

````
function Title({ render }) {
  return <div>{render()}</div>;
}
````
or
````
const Title = (props) => props.render();
````

The cool thing about render props, is that the component that receives the prop is very reusable. We can use it multiple times, passing different values to the render prop each time.

````
const Title = (props) => props.render();

render(
  <div className="App">
    <Title render={() => <h1>✨ First render prop! ✨</h1>} />
    <Title render={() => <h2>🔥 Second render prop! 🔥</h2>} />
    <Title render={() => <h3>🚀 Third render prop! 🚀</h3>} />
  </div>,
  document.getElementById("root")
);
````

The name render can be replaced and used more descriptive name

````
const Title = (props) => (
  <>
    {props.renderFirstComponent()}
    {props.renderSecondComponent()}
    {props.renderThirdComponent()}
  </>
);

render(
  <div className="App">
    <Title
      renderFirstComponent={() => <h1>✨ First render prop! ✨</h1>}
      renderSecondComponent={() => <h2>🔥 Second render prop! 🔥</h2>}
      renderThirdComponent={() => <h3>🚀 Third render prop! 🚀</h3>}
    />
  </div>,
  document.getElementById("root")
);
````
A component that takes a render prop usually does a lot more than simply invoking the render prop. 
Instead, we usually want to pass data from the component that takes the render prop, to the element that we pass as a render prop!

````
function Component(props) {
  const data = { ... }
  
  return props.render(data)
}

<Component render={data => <ChildComponent data={data} />}
````

Let's look into another example

````
function Input() {
  const [value, setValue] = useState("");

  return (
    <input
      type="text"
      value={value}
      onChange={e => setValue(e.target.value)}
      placeholder="Temp in °C"
    />
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>☃️ Temperature Converter 🌞</h1>
      <Input />
      <Kelvin />
      <Fahrenheit />
    </div>
  );
}

function Kelvin({ value = 0 }) {
  return <div className="temp">{value + 273.15}K</div>;
}

function Fahrenheit({ value = 0 }) {
  return <div className="temp">{(value * 9) / 5 + 32}°F</div>;
}
````

#### Lifting a state
One way to make the users input available to both the Fahrenheit and Kelvin component in the above example, we’d have to lift the state
Although this is a valid solution, it can be tricky to lift state in larger applications with components that handle many children. Each state change could cause a re-render of all the children, even the ones that don’t handle the data, which could negatively affect the performance of your app.


#### Render props

Instead, we can use render props! Let’s change the Input component in a way that it can receive render props.

````
function Input(props) {
  const [value, setValue] = useState("");

  return (
    <>
      <input
        type="text"
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Temp in °C"
      />
      {props.render(value)}
    </>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>☃️ Temperature Converter 🌞</h1>
      <Input
        render={(value) => (
          <>
            <Kelvin value={value} />
            <Fahrenheit value={value} />
          </>
        )}
      />
    </div>
  );
}
````

#### Children as a function

Besides regular JSX components, we can pass functions as children to React components. This function is available to us through the children prop, which is technically also a render prop.

````
export default function App() {
  return (
    <div className="App">
      <h1>☃️ Temperature Converter 🌞</h1>
      <Input>
        {(value) => (
          <>
            <Kelvin value={value} />
            <Fahrenheit value={value} />
          </>
        )}
      </Input>
    </div>
  );
}
````

````
function Input(props) {
  const [value, setValue] = useState("");

  return (
    <>
      <input
        type="text"
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Temp in °C"
      />
      {props.children(value)}
    </>
  );
}
````

### Pros
Sharing logic and data among several components is easy with the render props pattern. 

Components can be made very reusable, by using a render or children prop.

Although the Higher Order Component pattern mainly solves the same issues, namely reusability and sharing data, the render props pattern solves some of the issues we could encounter by using the HOC pattern.

We can separate our app’s logic from rendering components through render props. 
The stateful component that receives a render prop can pass the data onto stateless components, which merely render the data.

### Cons
The issues that we tried to solve with render props, have largely been replaced by React Hooks. As Hooks changed the way we can add reusability and data sharing to components, they can replace the render props pattern in many cases.

Since we can’t add lifecycle methods to a render prop, we can only use it on components that don’t need to alter the data they receive.
