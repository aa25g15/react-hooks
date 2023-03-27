# React Hooks

## useState Hook
* React manages application state and re-renders components based on state changes
* useState hook allows us to hook into this state that is managed by React and re-render the UI on state mutations
* If you just delcare a variable and try to change its value directly, it will change the value (regular JS) but it will not re-render the UI and reflect the changes
```jsx
import React, { useState } from "react";

const StateTutorial = () => {
  const [inputValue, setInputValue] = useState("Pedro");

  let onChange = (event) => {
    const newValue = event.target.value;
    setInputValue(newValue);
  };

  return (
    <div>
      <input placeholder="enter something..." onChange={onChange} />
      {inputValue}
    </div>
  );
};

export default StateTutorial;
```

## useReducer Hook
* The useState and useReducer hook have the same purpose, they both allow us to manage state and update the UI with state changes
* But useReducer is more useful for complex state, when multiple variables/state slices are changing, it is easier to make such large state changes using useReducer hook
```jsx
import React, { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1, showText: state.showText };
    case "toggleShowText":
      return { count: state.count, showText: !state.showText };
    default:
      return state;
  }
};

const ReducerTutorial = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0, showText: true });

  return (
    <div>
      <h1>{state.count}</h1>
      <button
        onClick={() => {
          dispatch({ type: "INCREMENT" });
          dispatch({ type: "toggleShowText" });
        }}
      >
        Click Here
      </button>

      {state.showText && <p>This is a text</p>}
    </div>
  );
};

export default ReducerTutorial;
```

## useEffect Hook
* useEffect hook is used to trigger an effect of a state change, like you have a count state which keeps track of a counter variable, and you want to call an API whenever the count changes, to do this, you will use useEffect with count in its dependency array
* An empty dependency array will trigger the useEffect once when component renders for the first time and NOT everytime (confirm this!)
* If you do not pass even the empty array and remove the argument altogether, the effect will be trigger on every component re-render (massive performance issues)
```jsx
import React, { useEffect, useState } from "react";
import axios from "axios";

function EffectTutorial() {
  const [data, setData] = useState("");
  const [count, setCount] = useState(0);

  useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/comments")
      .then((response) => {
        setData(response.data[0].email);
        console.log("API WAS CALLED");
      });
  }, [count]);

  return (
    <div>
      Hello World
      <h1>{data}</h1>
      <h1>{count}</h1>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        Click
      </button>
    </div>
  );
}

export default EffectTutorial;
```

## useRef
* In function components, we can use the useRef hook to create a value that doesn’t trigger a re-render when it’s changed, but we can use it to keep values between re-renders
* createRef on the other hand will create the ref again on every re-render
```jsx
import React, { useRef } from "react";

function RefTutorial() {
  const inputRef = useRef(null);

  const onClick = () => {
    inputRef.current.value = "";
  };
  return (
    <div>
      <h1>Pedro</h1>
      <input type="text" placeholder="Ex..." ref={inputRef} />
      <button onClick={onClick}>Change Name</button>
    </div>
  );
}

export default RefTutorial;
```

## useLayoutEffect
* This hook is called before the useEffect hook which is called when the component has been painted, this hook is called before the paint, so flash of wrong text/UI can be avoided
```jsx
import { useLayoutEffect, useEffect, useRef } from "react";

function LayoutEffectTutorial() {
  const inputRef = useRef(null);

  useLayoutEffect(() => {
    console.log(inputRef.current.value);
  }, []);

  useEffect(() => {
    inputRef.current.value = "HELLO"; // Before we set the value here to HELLO, we will see a flash of the default value PEDRO
  }, []);

  return (
    <div className="App">
      <input ref={inputRef} value="PEDRO" style={{ width: 400, height: 60 }} />
    </div>
  );
}

export default LayoutEffectTutorial;
```

## useImperativeHandle
* Can be used to define functions on the forwarded ref to a component such that those functions can be called inside the parents by invoking the functions on the ref, very useful for things like a snackbar, you do not need to pass down the functions from the parent to child anymore
  * Parent:
```jsx
import React, { useRef } from "react";
import Button from "./Button";

function ImperativeHandle() {
  const buttonRef = useRef(null);
  return (
    <div>
      <button
        onClick={() => {
          buttonRef.current.alterToggle();
        }}
      >
        Button From Parent
      </button>
      <Button ref={buttonRef} />
    </div>
  );
}

export default ImperativeHandle;
```
  * Child:
```jsx
import React, { forwardRef, useImperativeHandle, useState } from "react";

const Button = forwardRef((props, ref) => {
  const [toggle, setToggle] = useState(false);

  useImperativeHandle(ref, () => ({
    alterToggle() {
      setToggle(!toggle);
    },
  }));
  return (
    <>
      <button>Button From Child</button>
      {toggle && <span>Toggle</span>}
    </>
  );
});

export default Button;
```
