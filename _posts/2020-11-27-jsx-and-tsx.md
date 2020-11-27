---
layout: article
title: What is JSX and TSX
tags: 
    - React 
    - TypeScript
    - JavaScript
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: assets/images/posts/2020-11-27/cover.png
# LinkedIn Meta
linkedin: 
  title: What is JSX and TSX
  type: Article
  image: https://brunoprocopio.dev/assets/images/posts/2020-11-27/cover.png
  description: An introduction on using Typescript with React and how it's compiled to HTML.
  author: Bruno Procopio
---

An introduction on using Typescript with React and how it's compiled to HTML.

<!--more-->

We can face something like this when starting with React:

{% highlight javascript linenos %}
const element = <h1>Hello world</h1>;
{% endhighlight %}

If you're familiar with JavaScript, this is a very strange code. It contains **HTML** tags that don't feel like belonging in this code, but not so much out of the universe that it's clearly an error.

This is called a **syntax extension** to JavaScript, or how we'll call it from here onwards, **JSX**. On TypeScript it works in the same way, but we'll call it **TSX** (**T**ypeScript **S**yntax E**x**tension).

## Why JSX? ##

As we talked in our previous post, React create components to represent our [concerns](https://www.wikiwand.com/en/Separation_of_concerns). Instead of creating our UI in a separate file of our logic, React keep both of then in a single file, so that each if can represent a component. In resume, you should think that rendering logic is strongly coupled with the UI and the data, therefore it should be written on the same file.

Although, React doesn't necessary needs JSX files. You can write your code with just plain JavaScript/Typescript, but it's really unrecommended since JSX provides a visual aid and clearity on your code. You can check an example over [here](https://reactjs.org/docs/react-without-jsx.html).

## Let's get pratical ##

Ok, before we move to the next section to undestand how our JSX becomes HTML, let's start a new project that we'll use on every post, so we can build a real thing after all. I'll use here the [create-react-app](/2020/11/20/react-101.html) from our previous article and also [Chakra UI](https://chakra-ui.com/), which is a component library so we won't need to worry about styling and that sort of things. 

Chakra has an official create-react-app template, so let's use it:

``` bash
create-react-app chat-front-end --template @chakra-ui/typescript
```

**chat-front-end** is the name of our project. My goal here is to create a simple chat application, but by now it'll be just a login page.
{:.info}

I'll create a new file on /src called **Login.tsx**. This is my first component, and will be the first thing the user sees when he start the application. 

{% highlight tsx linenos %}
import * as React from "react"
import {
    Grid,
    HStack,
    Input,
    Button,
} from "@chakra-ui/react"

export const Login = () => (
    <Grid p={5} gap={1}>
        <HStack>
            <Input placeholder="Username" />
            <Input placeholder="Password" />
        </HStack>
        <Button justifySelf="flex-start">Login</Button>
    </Grid>
)
{% endhighlight %}

Let's understand what's happening here. This is a TypeScript (TSX) file, so what I'm doing is exporting my const **Login**. This will be a React object, an component. This component is composed of a master component **Grid**, with children inside of it which will be the actual implementation of what I want. It's actually invoking React to render my component like this, as a composition of those Chakra UI's components.

Now let's check what each of the components do:
- Grid: A simple component for a grid layout. More [info](https://chakra-ui.com/docs/layout/grid).
- HStack: Stack elements horizontally and apply space between them. Here I'm using it to stack my username and password inputs. More [info](https://chakra-ui.com/docs/layout/stack).
- Input: Textbox for inputing data from the user. More [info](https://chakra-ui.com/docs/form/input).
- Button: Used to trigger action, for example when the user clicks here. More [info](https://chakra-ui.com/docs/form/button).

With this set, we don't need to worry about styling or how our form will be rendered, so we can focus in our implementation. We're also using a third-party library for our components, so we have a generical way of using and reusing our components with ease. The next step is to use our newly created **Login** component. So edit the **App.tsx** to this:

{% highlight tsx linenos %}
import * as React from "react"
import { ChakraProvider } from "@chakra-ui/react"
import { Login } from "./Login"

export const App = () => (
  <ChakraProvider>
    <Login />
  </ChakraProvider>
)
{% endhighlight %}

Chakra UI needs this **ChackraProvider** to work properly, so set it at the root of our app. Then we use the Login component we just created. After that, just start the server and you should see the result, our login form: 

![Login form](/assets/images/posts/2020-11-27/login-form.png)

The best thing here is that we created a funcional form without worring about the UI, since everything is already implemented. It's even responsive, give it a try on your browser or cellphone. But how the TSX becomes HTML?

## Rendering elements ##

React has a key feature that's **React DOM**. It's responsible for handling all elements inside a **node**, which is a HTML element like a div, for example. If we take a look in our code, we can see it on **index.tsx**:

{% highlight tsx linenos %}
import { ColorModeScript } from "@chakra-ui/react"
import * as React from "react"
import ReactDOM from "react-dom"
import { App } from "./App"
import reportWebVitals from "./reportWebVitals"
import * as serviceWorker from "./serviceWorker"

ReactDOM.render(
  <React.StrictMode>
    <ColorModeScript />
    <App />
  </React.StrictMode>,
  document.getElementById("root"),
)

serviceWorker.unregister()
reportWebVitals()
{% endhighlight %}

Here, the React DOM is rendering our app on the element called **root**, which is a div on public/index.html. 

All elements rendered by React are **immutable**. That means that once it's created, it can not be changed by anyone, neither its children elements. So, if we need to update anything on the UI, we need to render it again with ReactDOM.render().

It doesn't look anything clever, indeed, but there are two points that make it works pretty nice:
1. We don't have to call ReactDOM.render() all the time, like we'll see when we get to component states and lifecycle. So, this will only be used one time in our app.
2. React DOM compares the new element with the previous one, and only updates the DOM on the elements that have really changed. Remember that updating DOM is a browser-heavy operation, so it's optimized with this point in mind.

Summing up, in React creators' words, *thinking about how the UI should look at any given moment, rather than how to change it over time, eliminates a whole class of bugs.*

Please take a look in the official docs for more info about [JSX](https://reactjs.org/docs/introducing-jsx.html) and about [rendering](https://reactjs.org/docs/rendering-elements.html). Thanks for reading and stay tunned!