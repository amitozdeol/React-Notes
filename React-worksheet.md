# React Worksheet
1. [ Introduction](#intro)
  1. [Intro to JSX](#introtojsx)
  2. [ReactDOM](#reactdom)
  3. [Hot module reloading](#hotmodulereloading)
  4. [Complex javascript in JSX](#complexjsinjsx)
2. [ Basics in React](#basicsinreact)
  1. [ES6 Object Initializer](#es6objectinitializer)
  2. [Unidirectional Data Flow](#unidirectionaldataflow)
  3. [Bindings](#bindings)
  4. [Event Handler](#eventhandler)

<a name="intro"></a>
## Introduction
<a name="introtojsx"></a>
### Intro to JSX -
JSX is neither a string nor HTML. It's a syntax extension to JavaScript. We recommend using it with React to describe what the UI should look like.

<u>Variable declaration and use-</u>

	var helloworld = "Welcome to the app";
	<div className="App">
		<h2>{helloworld}</h2>
	</div>
	 
	function formatName(user) {
	  return user.firstName + ' ' + user.lastName;
	}
	<div className="App">
		<h2>{formatName(user)}</h2>
	</div>
#### const & let
1. const = constant. cannot be re-assigned or re-declared.

2. let = can be re-assigned.

	// not allowed
	
	`const helloWorld = 'Welcome to the Road to learn React';`
	
	`helloWorld = 'Bye Bye React';`
	
	//allowed
	
	`let helloWorld = 'Welcome to the Road to learn React';`
	
	`helloWorld = 'Bye Bye React';`
	
<a name="reactdom"></a>
### ReactDOM
ReactDOM.render() uses a DOM node in your HTML to replace it with JSX.
**element** doesn't have to be a JSX component instance, it can also be simple HTML

element = `<h1>Hello react world</h1>`

```react
ReactDOM.render(
  element, //for rendering JSX
  document.getElementById('root') //place where react application hook into the HTML
);
```

#### Clock ticking example - 

	function tick() {
  		const element = (
    		<div>
      			<h1>Hello, world!</h1>
      			<h2>It is {new Date().toLocaleTimeString()}.</h2>
    		</div>
  		);
  		// highlight-next-line
  		ReactDOM.render(element, document.getElementById('root'));
​	}
​	setInterval(tick, 1000);
​	

	Output--
	Hello, world!
	It is 7:43:12 PM. // only the time get's updated, not any other HTML

<a name="hotmodulereloading"></a>
### Hot module replacement
Tool to reload the application in the browser without the page refresh. Add that in `src/App.js`

	if (module.hot) {
  		module.hot.accept();
​	}
​	
<a name="complexjsinjsx"></a>
### Complex javascript in JSX
Inside `src/App.js`

**Keys**: help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity

Data-

	const list = [
  	{
​    	title: 'React',
​    	url: 'https://reactjs.org/',
​    	author: 'Jordan Walke',
​    	num_comments: 3,
​    	points: 4,
​    	objectID: 0,
  	},
  	{
​    	title: 'Redux',
​    	url: 'https://redux.js.org/',
​    	author: 'Dan Abramov, Andrew Clark',
​    	num_comments: 2,
​    	points: 5,
​    	objectID: 1,
  	},
​	];

Code -

	class App extends Component {
  		render() {
​    		return (
​      			<div className="App">
​        			{list.map(function(item) {
​          				return (
​            				<div key={item.objectID}>
​              					<span>
​                					<a href={item.url}>{item.title}</a>
​              					</span>
​              					<span>{item.author}</span>
​              					<span>{item.num_comments}</span>
​              					<span>{item.points}</span>
​            				</div>
​          				);
​        			})}
​      			</div>
​    		);
  		}
​	}
​	
> Warning - When you don’t have stable IDs for rendered items, you may use the item index as a key as a last resort

##### Convert to react component
Convert the above code into a react component. This makes it easily reusable anywhere on the app.

    function ListAPI(props){
      const listItems = props.list.map((item) =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
      )
      return listItems;
    }
    
    class App extends Component {
      render() {
        return (
          <div className="App">
            <ListAPI list={list} />
          </div>
        );
      }
    }

Javascript array properties - [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

<a name="basicsinreact"></a>
## Basics in React
<a name="es6objectinitializer"></a>
### ES6 Object Initializer
1. Use shorthand notation to initialize the object, if the property name is same as variabe name.
	​	
		//ES5                                     //ES6
	​	const name = "Robin";                     const name = "Robin";
	​	const user = {               ==>          const user = {
	​	  name: name,                                name,
	​	};                                        };
	
2. **Method definition** - A property of an object can also refer to a function or a getter or setter method.

		var o = {
	​	  property: function (parameters) {},
	​	  get property() {},
	​	  set property(value) {}
	​	  
		  //can also do something like this
		  property(parameters) {},
    		  *generator() {}
	​	};
	​	
<a name="unidirectionaldataflow"></a>
### Unidirectional Data Flow
- constructor(props) => **Intialize state**
- this.setState()    => **Update state**
- Button click       => **View updated state**

I have a object called `items` with a list of arrays in it. To Intialize the state of this object in the Component:

	constructor(props){
		this.state = {
			initList: list
		}
	} 
To update the state when a button is clicked:

	<button
	  onClick={() =>
	    this.onDismiss(item.id)
	  } type="button">
	  Dismiss
	</button> 
	
	onDismiss(id){
	    const isNotId = item => {return item.id !== id};
	    const updatedList = this.state.initList.filter(isNotId);
	    //UPDATE STATE
	    this.setState({ list: updatedList });
	}

<a name="bindings"></a>
### Bindings
Before using `this` inside any class method, the method need to be bind to the class. Always bind the method inside the constructor, never anywhere else.

//correct

	constructor(){
	`this.function_name.bind(this)`	
	}

//wrong

	render() {
	    return (
	      <button
	        onClick={this.function_name.bind(this)}
	        type="button"
	      >
	        Click Me
	      </button>
	    );
	}

This is because every time the render method is called(component is updated), it'll bind the method which will affect the application performance.

#### Shortcut approach
To avoid repetitive binding inside the constructor for each method, use Javascript ES6 approach when creating method: 

function_name = () => {
​    console.log(this);
  } 

[https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax](https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax)

<a name="eventhandler"></a>

### Event Handler
