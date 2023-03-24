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
  }, []);

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
