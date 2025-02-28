# Advanced React Concepts & Best Practices

## Overview
This document covers essential advanced React concepts, including effects, refs, context, portals, suspense, and error boundaries. Understanding these topics will help you build scalable, maintainable, and high-performance React applications.

## Table of Contents
- [Effects & Side Effects (`useEffect`)](#effects--side-effects-useeffect)
- [Refs & DOM Manipulation (`useRef`)](#refs--dom-manipulation-useref)
- [Context API for Global State Management](#context-api-for-global-state-management)
- [Portals for Rendering Outside the Component Tree](#portals-for-rendering-outside-the-component-tree)
- [Suspense for Data Fetching & Lazy Loading](#suspense-for-data-fetching--lazy-loading)
- [Error Boundaries for Stability](#error-boundaries-for-stability)
- [Further Learning](#further-learning)

---

## Effects & Side Effects (`useEffect`)

### What Are Effects?
Effects in React allow components to interact with external systems, such as:
- Communicating with a server via API requests
- Interacting with browser APIs (e.g., `localStorage`, `document.title`)
- Managing subscriptions and event listeners

### When to Use `useEffect`
Use the `useEffect` hook when you need to run code **outside of React’s rendering cycle**, for example:
- **Fetching data when a component loads**
- **Subscribing to external services**
- **Manually changing the DOM**

```jsx
import { useEffect, useState } from "react";

function FetchData() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("https://api.example.com/data")
      .then(response => response.json())
      .then(data => setData(data));
  }, []); // Runs once when the component mounts

  return <div>{data ? JSON.stringify(data) : "Loading..."}</div>;
}
```

### Cleanup in `useEffect`
If an effect creates a **subscription** or a **side effect** that needs cleanup, return a function inside `useEffect`:

```jsx
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener("resize", handleResize);

  return () => window.removeEventListener("resize", handleResize);
}, []);
```

---

## Refs & DOM Manipulation (`useRef`)

### Why Use Refs?
Refs allow direct access to **DOM elements** and **persist values** without triggering re-renders. This is useful for:
- **Managing focus** (e.g., auto-focusing an input field)
- **Storing mutable values**
- **Interacting with third-party libraries**

### Example: Auto-Focusing an Input Field
```jsx
import { useRef, useEffect } from "react";

function InputFocus() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} placeholder="Type here..." />;
}
```

---

## Context API for Global State Management

### Why Context?
The **Context API** helps pass data deep into the component tree **without prop drilling**. This is useful for **global state management**, like user authentication or theme settings.

### How to Use Context
1. **Create Context**
2. **Provide Context**
3. **Consume Context**

```jsx
import { createContext, useContext } from "react";

const ThemeContext = createContext("light");

function ThemeProvider({ children }) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>;
}

function ThemeSwitcher() {
  const theme = useContext(ThemeContext);
  return <div>Current Theme: {theme}</div>;
}
```

---

## Portals for Rendering Outside the Component Tree

### Why Use Portals?
Portals allow rendering components **outside the main DOM hierarchy**. This is useful for:
- **Modals**
- **Tooltips**
- **Dropdowns**

### Example: Creating a Portal
```jsx
import ReactDOM from "react-dom";

function Modal({ children }) {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById("modal-root")
  );
}
```

---

## Suspense for Data Fetching & Lazy Loading

### What is Suspense?
Suspense lets you **delay rendering** until data is ready or a component is loaded asynchronously.

### Example: Lazy Loading a Component
```jsx
import { lazy, Suspense } from "react";

const LazyComponent = lazy(() => import("./LazyComponent"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

---

## Error Boundaries for Stability

### Why Use Error Boundaries?
React apps **crash** when encountering JavaScript errors. Error boundaries **catch** errors and display a fallback UI instead of breaking the whole app.

### Creating an Error Boundary
```jsx
import React, { Component } from "react";

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error("Error caught:", error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

---

## Further Learning
For a **deep dive into React**, consider:
- **Official React Documentation**: [https://react.dev/](https://react.dev/)
- **React Bootcamps & Courses**
- **Building Projects** to practice real-world implementations

Mastering these concepts will help you build high-quality React applications with best practices. 🚀
