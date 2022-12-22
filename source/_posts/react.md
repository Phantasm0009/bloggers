---
title: React.js - Custom Hooks
date: 2022-12-19 14:56:06
tags:
    - React
---
![$cover](images/react.webp)

The topic of hooks in React development is quite popular and has been analyzed more than once by various specialists. The purpose of our material is not so much to bring something new as to analyze a sufficiently important and relevant topic in the most accessible and understandable language, so that it does not cause difficulties even for beginners in the development of applications on React.

First appeared in React version 16.8. The structure of applications based on the use of hooks was very pleasant to the community for its flexibility and simplicity, which allowed it to practically replace classes.

[](#its-easier-with-hooks)It's easier with hooks!
-------------------------------------------------

Many newcomers were scared away by the classes with their cumbersomeness and less transparency. The mechanisms of state and reusable use looked much more complicated than in reality. The introduction of hooks has largely solved this problem and significantly increased the popularity of the library, which has been steadily growing for several years.

[](#reacts-main-hooks-out-of-the-box)React's main hooks out of the box
----------------------------------------------------------------------

React out of the box contains several very important hooks.

### [](#usestate-manage-the-state-in-functional-components)useState - manage the state in functional components

One of the most important hooks is useState. Its name immediately makes it clear what it is used for - it is responsible for the state. This hook allows you to influence the state like the this.setState() method. First of all, let's look at a simple example of its use:  

    const MyComponent = () => {
      const [counter, setCounter] = useState(1);
      return (
        <button onClick={() => setCounter(counter + 1)}>
          +1 to the previous value
        </button>
    );
    };
    

Enter fullscreen mode Exit fullscreen mode

In this example, after the functional element is declared, we call the useState method, passing it the default value:  

    const [counter, setCounter] = useState(1);
    

Enter fullscreen mode Exit fullscreen mode

The method returns an array of 2 elements. The 1st element is a state variable, the 2nd element contains a function to change this state. The button is set to the onClick event, which allows you to change the state by increasing the value by 1 from the previous one:  

    onClick={() => setCounter(counter + 1)}
    

Enter fullscreen mode Exit fullscreen mode

### [](#usecontext-we-transfer-information-to-any-levels-of-nesting)useContext - we transfer information to any levels of nesting

As a rule, the parent component shares data with children using props. However, there is a need to transfer this data not only to the nearest "children" of this component, but also to more nested components. Transmitting "relatives" along the entire chain is terribly inconvenient and can lead to mistakes. In this case, it is convenient to use useContext.  

    import {createContext, useContext} from "react";
    
    const MyContext = createContext("no data");
    
    const Bookcase = () =⟩ {
      return (
        ⟨MyContext.Provider value="шкаф #1 "⟩
          ⟨Bookshelf /⟩
        ⟨/MyContext.Provider⟩
      );
    };
    
    const Bookshelf = () =⟩ {
      return ⟨Book /⟩;
    };
    
    const Book = () =⟩ {
      const context = useContext(MyContext);
      return `Book in is "${context}"`;
    };
    

Enter fullscreen mode Exit fullscreen mode

In this example, thanks to the useContext hook, each book can have information about which cabinet it is located in, and there is no need to receive it from the direct parent (book shelf).

### [](#useeffect-we-implement-lifecycle-functionality)useEffect - we implement lifecycle functionality

Used to simulate the lifecycle, as well as the componentDidMount, componentDidUpdate, componentWillUnmount methods do when using the React structure on classes. A callback function is passed as arguments, which performs the necessary actions and an array with variables, the change of which must be monitored and callback when they are changed.

### [](#useref-link-variables-directly-to-dom)useRef - link variables directly to DOM

To access DOM elements directly in functional components, you must use useRef. In the example below, we bind the button to the buttonRef variable and can use this link in the callback function passed as the 1st argument in useRef.  

    const MyButton = () =⟩ {
      const buttonRef = useRef();
      useEffect(() =⟩ {
        console.log(buttonRef.current.innerHTML);
      }, []);
      return ⟨button ref={ref}⟩My button⟨/button⟩;
    };
    

Enter fullscreen mode Exit fullscreen mode

[](#more-hooks)More hooks!
--------------------------

useReducer: allows you to store the status value regardless of nesting, is an analogue of Redux.

useMemo: allows you to store the value and recalculate only when constraints change.

useCallback: used to memoize functions, avoids unnecessary rendering and re-creation of functions. Very useful for optimization.

[](#were-writing-our-own-hook)We're writing our own hook
--------------------------------------------------------

In essence, hooks are common functions, the name of which begins with the prefix "use". They can use other hooks, accept arguments and return the result.

It is convenient to create your own hooks when you need to learn the logic that is often used in different components of the application. It is necessary to remember about important restrictions for hooks: you can't call inside conditional constructions and loops - this will cause an error in the application.

Let's look at an example from the official react documentation. It is interesting because it describes an example of creating a useReducer hook using an existing useState.

Let's say we work with a component that has a fairly developed state management logic depending on their type. In this case, useState is not very convenient to use with centralized state management logic and Redux-reducer is more suitable:  

    function todosReducer(state, action) {
      switch (action.type) {
        case 'add':
          return [...state, {
            text: action.text,
            completed: false
          }];
        // ... other actions ...
        default:
          return state;
      }
    }
    

Enter fullscreen mode Exit fullscreen mode

Our hook will look like this (simplified):  

    function useReducer(reducer, initialState) {
      const [state, setState] = useState(initialState)
    
      function dispatch(action) {
        const nextState = reducer(state, action)
        setState(nextState)
      }
    
      return [state, dispatch]
    }
    

Enter fullscreen mode Exit fullscreen mode

Here we accept as parameters - a reworker function responsible for the log of state change, initial state, and return the current state and function to change it. At the same time, we can very easily modify the logic of changing states by adding/changing types in the todosReducer function. After that, it is convenient to use this logic in your component:  

    function Todos() {
      const [todos, dispatch] = useReducer(todosReducer, [])
      function handleAddClick(text) {
        dispatch({ type: 'add', text })
      }
      // ...
    }
    

Enter fullscreen mode Exit fullscreen mode

Hooks can be used for a wide variety of purposes. Let's look at an example of a hook for working with local browser storage:  

    function getStorageValue(key, defaultValue) {
      const saved = localStorage.getItem(key);
      const initial = !saved || saved === 'undefined' ? null : JSON.parse(saved);
      return initial || defaultValue;
    }
    export default  function useLocalStorage (key, defaultValue){
      const [value, setValue] = useState(() =⟩ {
        return getStorageValue(key, defaultValue);
      });
    
      useEffect(() =⟩ {
        localStorage.setItem(key, JSON.stringify(value));
      }, [key, value]);
    
      return [value, setValue];
    };
    

Enter fullscreen mode Exit fullscreen mode

We defined the function of working with localStorage by writing our own logic for obtaining primary data from the repository. Then we created a hook that initializes the state using the result of this function. We also use useEffect to write to localStorage when changing the value of the key or value variables. In our component, we can use the hook in this way:  

    const [storageData, setStorageData] = useLocalStorage('my-data');
    

Enter fullscreen mode Exit fullscreen mode

So, we managed to link data in the state with the data in the local browser storage.

That's all for now, I hope this article helped make understanding the topic of hooks easier for you!

Thanks for reading. Leave your comments below and follow my [GitHub](https://github.com/Phantasm0009) <3