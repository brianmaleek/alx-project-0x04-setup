# StateCraft: Mastering State Management with Redux & Context API

## Project Description

This project series demonstrates different approaches to state management in React applications by building an interactive counter application. Starting with React’s built-in useState hook, we progressively implement more sophisticated state management solutions including Context API and Redux. The project showcases how to share state across multiple components and maintain application-wide data consistency.

## Learning Objectives

By completing these projects, you will:

1. Understand fundamental React state management using useState
2. Learn to implement global state management with Context API
3. Master Redux for complex state management scenarios
4. Compare different state management solutions
5. Implement state persistence across components
6. Understand the concept of single source of truth
7. Learn to structure applications for scalable state management

## Requirements

### Technical Requirements

- Node.js (v14 or later)
- npm or yarn package manager
- React (v18 or later)
- TypeScript
- Next.js framework
- Redux Toolkit (for the Redux implementation)
- React-Redux bindings

## Development Environment

- Code editor (VS Code recommended)
- Terminal/command line access
- Modern web browser (Chrome, Firefox, or Edge)

## Best Practices

### General React Practices

1. **Component Organization**: Keep components small and focused
2. **Type Safety**: Utilize TypeScript for type checking
3. **Separation of Concerns**: Separate state management from UI components
4. **Immutability**: Always treat state as immutable
5. **Single Responsibility**: Each component/file should have one primary responsibility

## State Management Specific

1. **Context API**:

    - Create context providers at the appropriate level in the component tree
    - Use custom hooks for context consumption
    - Provide proper TypeScript interfaces for context values

2. **Redux**:

    - Follow Redux Toolkit’s recommended patterns
    - Use slices for modular state management
    - Type your Redux store and actions
    - Create typed hooks for dispatch and selector usage

3. **Performance**:

    - Memoize selectors when necessary
    - Avoid unnecessary re-renders with proper state selection
    - Consider using Redux middleware for complex side effects

## Project Structure

### Common Files

- `pages/`: Contains page components
  - `counter-app.tsx`: Main counter application
- `components/`: Reusable UI components
  - `layouts/`: Application layout components
  - `Header.tsx`: Shared header component

## Variant-Specific Files

1. **useState Version (0x04)**

    - Simple state management within a single component

2. **Context API Version (0x05)**

    - `context/CountContext.tsx`: Context provider and hooks
    - Modified `_app.tsx` to wrap application with provider

3. **Redux Version (0x06)**

    - `store/store.ts`: Redux store configuration
    - Updated components to use Redux hooks

## Expected Outcomes

After completing all versions, you will have: 1. A working counter application with three different state management implementations 2. Understanding of when to use each state management solution 3. Practical experience with modern React state management patterns 4. A foundation for building more complex stateful applications 5. Ability to make informed decisions about state management in your projects

⚠️ Note:
While copying and pasting code may seem quick and convenient, it often hinders true understanding. To get the most out of this learning experience, we strongly recommend that you:

Carefully read and understand the instructions for each task.
Type the code yourself to internalize the logic and structure.
Experiment and test your code to see how it works in practice.
Hands-on practice leads to deeper learning and long-term retention. Keep coding!

#### 📝 Project Assessment (Hybrid)

Your project will be evaluated primarily through manual reviews. To ensure you receive your full score, please:

✅ Complete your project on time
📄 Submit all required files
🔗 Generate your review link
👥 Have your peers review your work

An auto-check will also be in place to verify the presence of core files needed for manual review.

⏰ Important Note
If the deadline passes, you won’t be able to generate your review link—so be sure to submit on time!

## Tasks

### 0. Adding a counter app to our platform with useState Hook

**mandatory**

**Objectives**: We are going to modify our project to include actual functionalities. Duplicate this project alx-project-0x03 with a new name of alx-project-0x04. The Counter App uses react useState hook, to maintain an initial state of your count. This helps keep tracking of the counts, and manages the behavior of the counting effect.

**Instructions**:

- Create a new file pages/counter-app.tsx
- Replace the content of this file with the code below

```typescript
import { useState } from 'react';

const CounterApp: React.FC = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count > 0 ? count - 1 : 0);
  };

  return (
    <div className="min-h-screen bg-gradient-to-r from-yellow-400 to-pink-500 flex flex-col justify-center items-center text-white">
      {/* Title */}
      <h1 className="text-6xl font-extrabold mb-6">🤖 Fun Counter App 🎉</h1>

      {/* Funny message */}
      <p className="text-lg font-medium mb-4">
        Current count: {count} {count === 0 ? "🙈 No clicks yet!" : count % 10 === 0 && count !== 0 ? "🔥 You're on fire!" : ""}
      </p>

      {/* Counter Display */}
      <div className="text-6xl font-bold mb-8">
        {count}
      </div>

      {/* Buttons */}
      <div className="flex space-x-4">
        <button
          onClick={increment}
          className="bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Increment 🚀
        </button>
        <button
          onClick={decrement}
          className="bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Decrement 👎
        </button>
      </div>

      {/* Footer message */}
      <p className="mt-8 text-sm text-white opacity-75">
        Keep clicking, who knows what happens at 100? 😏
      </p>
    </div>
  );
}

export default CounterApp;
```

- Save and close your files
- Run `npm run dev -- -p 3000` from the terminal
- From a tab in your browser type `http://localhost:3000` to see the changes made.
- Click on the Counter App button from the home screen.
- Play around with the button and observe the behavior

**Repo**:

- **GitHub repository**: **alx-project-0x04-setup**
- **Directory**: **alx-project-0x04**
- **File**: [pages/counter-app.tsx](./alx-project-0x04/pages/counter-app.tsx)

### 1. Include a ContextAPI for global state management

**mandatory**

**Objective**: ContextAPI allows us to maintain a global state for our applications that can be used across multiple components without prop drilling. For our application, we have a Header component, which is completely different from our CounterApp component (mapped to the route /counter-app). We want to achieve a feature such that when a button is clicked in one component, the effect can reflect in another component. E.g: Our counter will reflect in both Header and CounterApp Components.

**Instructions**:

- Duplicate this project alx-project-0x04 with a new name of alx-project-0x05.
- Create a new directory `context/` in the project root directory
- Create an empty file CountContext.tsx under the `context/` directory
- Replace the content of the CountContext.tsx with the following

```typescript
import { createContext, useContext,  useState, ReactNode } from "react"

interface CountContextProps {
  count: number
  increment: () => void
  decrement: () => void
}

export const CountContext = createContext<CountContextProps | undefined>(undefined)

export const CountProvider = ({ children }: { children: ReactNode}) => {

  const [count, setCount] = useState<number>(0)

  const increment = () => setCount((count ) =>count + 1)
  const decrement = () => setCount((count) => count > 0 ? count - 1 : 0)

  return (
    <CountContext.Provider value={{ count, increment, decrement }}>
      {children}
    </CountContext.Provider>
  )
}



export const useCount = () => {
  const context = useContext(CountContext)

  if (!context) {
    throw new Error("useCount must be within a Count Provider")
  }

  return context
}
```

- Modify your App Document (_app.tsx) to look like this

```typescript
import Layout from "@/components/layouts/Layout";
import "@/styles/globals.css";
import type { AppProps } from "next/app";
import { CountProvider } from "@/context/CountContext";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <CountProvider>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </CountProvider>
  )
}
```

- Modify your components/layouts/Header.tsx to look like this:

```typescript
import Link from "next/link";
import Button from "../common/Button";
import { usePathname } from "next/navigation";
import { useCount } from "@/context/CountContext"";

const Header: React.FC = () => {

  const pathname = usePathname()
  const { count } = useCount()

  return (
    <header className="fixed w-full bg-white shadow-md">
      <div className="container mx-auto flex justify-between items-center py-6 px-4 md:px-8">
        <Link href="/" className="text-3xl md:text-5xl font-bold text-gray-800 tracking-tight">
          Splash App
        </Link>

        {/* Button Group */}
        <div className="flex gap-4">
          {
            !["/counter-app"].includes(pathname) ? (
              <>
              <Button
            buttonLabel="Sign In"
            buttonBackgroundColor="red"
          />
          <Button
            buttonLabel="Sign Up"
            buttonBackgroundColor="blue"
          /></>
            ) : (
              <p className=" font-semibold text-lg">Current count : {count}</p>
            )
          }

        </div>
      </div>
    </header>
  );
};

export default Header;
```

- Save and close your files
- Run `npm run dev -- -p 3000` from the terminal
- From a tab in your browser type `http://localhost:3000` to see the changes made.
- Click on the Counter App button from the home screen.
- Play around with the button and observe the behavior

**Repo**:

- **GitHub repository**: **alx-project-0x04-setup**
- **Directory**: **alx-project-0x05**
- **File**: [context/CountContext.tsx](alx-project-0x05/context/CountContext.tsx), [/components/layout/Header.tsx](alx-project-0x05/components/layout/Header.tsx), [pages/_app.tsx](alx-project-0x05/pages/_app.tsx)

### 2. Include a Redux for global state management

**mandatory**

**Objective**: Redux allows us to maintain a global state for our applications that can be used across multiple components without prop drilling just like ContextAPI, but for large scale applications . For our application, we have a Header component, which is completely different from our CounterApp component (mapped to the route /counter-app). We want to achieve a feature such that when a button is clicked in one component, the effect can reflect in another component. E.g: Our counter will reflect in both Header and CounterApp Components.

**Instructions**:

- Duplicate this project alx-project-0x05 with a new name of alx-project-0x06.
- Install redux using the following command

```typescript
npm install redux react-redux @reduxjs/toolkit
npm install @types/react-redux
```

- Create a new directory store/ in the project root directory
- Create an empty file store.ts under the store/ directory
- Replace the content of the store.ts with the following

```typescript
import { configureStore, createSlice  } from "@reduxjs/toolkit";
import { useDispatch } from "react-redux";

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: (state) =>{
      state.value += 1
    },
    decrement: (state) => {
      state.value > 0 ? state.value -= 1 : 0
    }
  }
});


const store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }
})


export const { increment, decrement } = counterSlice.actions

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch

export const useAppDispatch = () => useDispatch<AppDispatch>()

export default store
```

- Update the content of `pages/counter-app.tsx` to look like this

```typescript
import { useSelector } from "react-redux";
import { RootState, useAppDispatch, AppDispatch, increment, decrement } from "@/store/store";

const CounterApp: React.FC = () => {

  const count = useSelector((state: RootState) => state.counter.value)
  const dispatch: AppDispatch = useAppDispatch()


  return (
    <div className="min-h-screen bg-gradient-to-r from-yellow-400 to-pink-500 flex flex-col justify-center items-center text-white">
      {/* Title */}
      <h1 className="text-6xl font-extrabold mb-6">🤖 Fun Counter App 🎉</h1>

      {/* Funny message */}
      <p className="text-lg font-medium mb-4">
        Current count: {count} {count === 0 ? "🙈 No clicks yet!" : count % 10 === 0 && count !== 0 ? "🔥 You're on fire!" : ""}
      </p>

      {/* Counter Display */}
      <div className="text-6xl font-bold mb-8">
        {count}
      </div>

      {/* Buttons */}
      <div className="flex space-x-4">
        <button
          onClick={() => dispatch(increment())}
          className="bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Increment 🚀
        </button>
        <button
          onClick={() => dispatch(decrement())}
          className="bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Decrement 👎
        </button>
      </div>

      {/* Footer message */}
      <p className="mt-8 text-sm text-white opacity-75">
        Keep clicking, who knows what happens at 100? 😏
      </p>
    </div>
  );
}

export default CounterApp;
```

- Update the content of components/layouts/Header.tsx to look like this

```typescript
import Link from "next/link";
import Button from "../common/Button";
import { usePathname } from "next/navigation";
import { RootState } from "@/store/store";
import { useSelector } from "react-redux";

const Header: React.FC = () => {

  const pathname = usePathname()
  const count = useSelector((state: RootState) => state.counter.value)


  return (
    <header className="fixed w-full bg-white shadow-md">
      <div className="container mx-auto flex justify-between items-center py-6 px-4 md:px-8">
        <Link href="/" className="text-3xl md:text-5xl font-bold text-gray-800 tracking-tight">
          Splash App
        </Link>

        {/* Button Group */}
        <div className="flex gap-4">
          {
            !["/counter-app"].includes(pathname) ? (
              <>
              <Button
            buttonLabel="Sign In"
            buttonBackgroundColor="red"
          />
          <Button
            buttonLabel="Sign Up"
            buttonBackgroundColor="blue"
          /></>
            ) : (
              <p className=" font-semibold text-lg">Current count : {count}</p>
            )
          }

        </div>
      </div>
    </header>
  );
};

export default Header;
```

- Save and close your files
- Run `npm run dev -- -p 3000` from the terminal
- From a tab in your browser type `http://localhost:3000` to see the changes made.
- Click on the Counter App button from the home screen.
- Play around with the button and observe the behavior

**Repo**:

- **GitHub repository**: **alx-project-0x04-setup**
- **Directory**: **alx-project-0x06**
- **File**: [store/store.ts](./alx-project-0x06/store/store.ts), [components/layout/Header.tsx](./alx-project-0x06/components/layout/Header.tsx), [pages/counter-app.tsx](./alx-project-0x06/pages/counter-app.tsx)
