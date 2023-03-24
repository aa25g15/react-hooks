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
