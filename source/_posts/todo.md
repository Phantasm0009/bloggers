---
title: How to Build a Todo App With React and Firebase Database
date: 2022-12-16 20:00:19
tags:
    - React
    - Firebase
---
[How to build a todo app with React and Firebase Database (3 Part Series)](/rossanodan/series/1157)
---------------------------------------------------------------------------------------------------

[1 How to build a todo app with React and Firebase Database](/rossanodan/how-to-build-a-todo-app-with-react-and-firebase-database-1kik "Published Jun 11 '19") [2 How to build a todo app with React and Firebase Database](/rossanodan/how-to-build-a-todo-app-with-react-and-firebase-database-coh "Published Jun 11 '19") [3 How to build a todo app with React and Firebase Database](/rossanodan/how-to-build-a-todo-app-with-react-and-firebase-database-3gmh "Published Jun 28 '19")

[](#day-1)Day 1
---------------

📅 11-06-2019  
🕐 1h  
🏁 Initial setup and getting ready

[](#initial-setup)Initial setup
===============================

I’m going to use `create-react-app` tool to scaffold the project folder. It’s a tool provided by Facebook that allow to easy scaffold a pre-configured starter project.

    npx create-react-app todo-app
    

The initial project consists of

*   `node_modules`: contains all necessary dependencies. It’s generated scaffolding the app with `create-react-app` tool (there’s a `npm install` into it)
*   `public`: contains few files like the `index.html`, the `application favicon` and a `manifest` that contains few basic application settings
*   `src`: contains the code
*   `.gitignore`
*   `package.json`: there are all the project information like the version, the author and mainly the dependencies the application needs to work properly
*   `yarn.lock`: contains all the dependencies Yarn needs with relative versions

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--8RpZVUt0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ac9cpb4bpprx03cm8euv.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--8RpZVUt0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ac9cpb4bpprx03cm8euv.png)

To run the starter basic application it’s enough to do

    cd todo-app
    npm start
    

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--SAZJSz6j--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/r7rniixpbddwys3hx554.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--SAZJSz6j--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/r7rniixpbddwys3hx554.png)

`npm start` is one of several pre-configured commands I’m going to use to develop this application. Other commands are:

*   `npm test`
*   `npm build`
*   `npm eject` (stay away from it for now)

[](#get-ready-for-components)Get ready for components
=====================================================

In order to work with a sustainable and scalable structure, I like to keep things separated. I’m going to create two folders for components.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--TvKQxUZr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ty6xjeagmabliffuwtvi.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--TvKQxUZr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ty6xjeagmabliffuwtvi.png)

These two folders will contains (surprise) components!  
The only difference between them is that a **container** is a component that **manages the application state** so it’s a **stateful component**. Other components are **stateless components**.

[](#the-main-component-raw-ltapp-gt-endraw-)The main component `<App />`
========================================================================

Let’s create the first component.  
I’m going go to move the `App.js`, `App.test.js` and `App.css` into their own folder `./containers/App/`:

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--HAJq00-D--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/hm7jg5fkc8x2vifhr40h.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--HAJq00-D--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/hm7jg5fkc8x2vifhr40h.png)

    // App.js
    import React, { Component } from 'react';
    import './App.css';
    
    class App extends Component {
      render() {
        return (
          <div className="App">
            Placeholder
          </div>
        );
      }
    }
    
    export default App;
    

    /* App.css */
    .App {
      text-align: center;
    }
    

No changes to the `App.test.js` at the moment.

Update `index.js` - importing App component - because files location is changed and delete useless files like `logo.svg`.

[](#the-raw-lttodo-gt-endraw-component)The `<Todo />` component
===============================================================

Let’s create the `<Todo />` component into the `./components` folder. Create `Todo.js`, `Todo.test.js` and `Todo.css`.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--cJb-pF1e--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/1z32qe9hv618920cfdwx.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--cJb-pF1e--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/1z32qe9hv618920cfdwx.png)

    // Todo.js
    import React from 'react';
    import './Todo.css';
    
    const todo = () => (
        <div className="Todo">
            <p>Placeholder</p>
        </div>
    )
    
    export default todo;
    

    /* Todo.css */
    .Todo {} /* Empty for now */
    

`Todo.test.js` is similar to `App.test.js`:

    import React from 'react';
    import ReactDOM from 'react-dom';
    import Todo from './Todo';
    
    it('renders without crashing', () => {
      const div = document.createElement('div');
      ReactDOM.render(<Todo />, div);
      ReactDOM.unmountComponentAtNode(div);
    });
    

Now I can use the `<Todo />` component into the `<App />` component, doing:

    import React, { Component } from 'react';
    import './App.css';
    
    import Todo from '../../components/Todo/Todo';
    
    class App extends Component {
      render() {
        return (
          <div className="App">
            <Todo />
          </div>
        );
      }
    }
    
    export default App;
    
    

Check out the code here  

![GitHub logo](https://res.cloudinary.com/practicaldev/image/fetch/s--qF2jUiUG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://practicaldev-herokuapp-com.freetls.fastly.net/assets/github-logo-6a5bca60a4ebf959a6df7f08217acd07ac2bc285164fae041eacb8a148b1bab9.svg) [rossanodan](https://github.com/Phantasm0009) / [todo-app](https://github.com/Phantasm0009/TodoList)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Simple to-do app built with React.

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

How to run locally
------------------

    git clone https://github.com/Phantasm0009/TodoList.git
    cd todo-app
    npm install
    npm start
    

  

[View on GitHub](https://github.com/Phantasm0009/TodoList)

  

  

---------------------------------------------------------------------------------------------------
