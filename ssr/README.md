# Server Side Rendering

## How SSR works

Whenever you visit a website, your browser makes a request to the server that contains the contents of the website. Once the request is done processing, your browser gets back the fully rendered HTML and displays it on the screen. If you then decide to visit a different page on the website, your browser will once again make another request for the new information. This will occur each and every time you visit a page that your browser does not have a cached version of.

## How CSR works

When developers talk about client-side rendering, they’re talking about rendering content in the browser using JavaScript. So instead of getting all of the content from the HTML document itself, you are getting a bare-bones HTML document with a JavaScript file that will render the rest of the site using the browser.

## Pros and cons

<img src="ssr_pros_cons.png" width="500px"/>

## Server side rendering in React

We use the method `renderToString` from `react-dom`.

All we're doing is converting our React app to string.

We render the app on the server with robots already created. So it will appear a lot faster.

We need React on the browser and the server.

We use these methods :

<img src="react_dom.png" width="500px"/>

`ReactDOM.hydrate()` is like calling the render method except it preserve markup and only attach event listeners, or any JS functionalities. Otherwise, there will be just text displayed.

To sum up, `hydrating` makes the page interactive.

`ReactDOMServer.renderToNodeStream` : similar to `renderToString` except it uses nodes stream that makes the process faster.

## Why CSR has a low SEO potential

We just render a simple HTML file. When the crawlers programs look at the webpages, it only sees something like that :

`<div id="root"></div>`

We can improve the SEO by putting a `meta` description tag but it won't be enough. Furthermore, if we're doing AJAX requests at the initial load, the rendering will be slower.

So if we need a website that depends on SEO (hotel companies for example), we need SSR because the content is presented before we actually get it in the view.

## SSR React libraries

- [Gatsby](https://www.gatsbyjs.org/) : Great for static pages, documentations. React documentation uses Gatsby !
- [Next](https://nextjs.org/learn/) : For apps beyond static pages. Easy to use and one of the best implementation of SSR. They include code splitting, SEO optimization, ...

## Setting up Next.js

Includes :

- Routing
- Webpack set up
- Use different types of CSS
- Support for TypeScript
- etc.

We need a `pages` folder at the root.

## DOM differences

### Client-side

<img src="robofriends_dom.png" width=500px>

### Server-side

<img src="next_dom.png" width=500px>

## Basic Next.js app

`package.json`

```json
{
  "name": "next-ssr",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "start": "next"
  },
  "dependencies": {
    "next": "^6.1.1",
    "react": "^16.4.1",
    "react-dom": "^16.4.1"
  }
}
```

`pages/Image.js` 

```js
const Image = () => (
  <img src="https://pbs.twimg.com/media/DfkhrO1XUAEYkdw.jpg"/>
);

export default Image;
```

`pages/index.js`

```js
// Allows to do client-side routing (check the network tab in Chrome)
import Link from "next/link";
import Robots from "./robots";

const Index = () => (
  <div>
    <h1>SSR Magician</h1>
    <Link href="/about">
      <button>About</button>
    </Link>
    <Link href="/robots">
      <button>Robots</button>
    </Link>
    {/* <a href="/about">About</a> */}
  </div>
);

export default Index;
```

`pages/about.js`

```js
import Link from "next/link";
import Image from "../components/Image";

const About = () => (
  <div style={{ fontSize: "20px", color: "blue" }}>
    <h1>About</h1>
    <Link href="/">
      <button>Back</button>
    </Link>
    <Image />
    <p>I was a magician once</p>
  </div>
);

export default About;
```

`pages/robots.js`

```js
import Link from "next/link";
import fetch from "isomorphic-unfetch";

const Robots = (props) => (
  <div>
    <h1>Robots</h1>
    <Link href="/">
      <button>Home</button>
    </Link>
    <div>
      {
        props.robots.map(robot => (
          <li key={robot.id}>
          <Link href={`robot/${robot.id}`}>
            <a>{robot.name}</a>
          </Link>
          </li>
        ))
      }
    </div>
  </div>
);

Robots.getInitialProps = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json()
  return {
    robots: data
  }
}

export default Robots;
```

## Server-side vs client-side routing

Routing is the mechanism by which requests are connected to some code. It is essentially the way you navigate through a website or web-application. By clicking on a link, the URL changes which provides the user with some new data or a new webpage.

A server-side request causes the whole page to refresh. This is because a new GET request is sent to the server which responds with a new document, completely discarding the old page altogether.

**Pros of server-side routing :**
- Only request the data that's needed
- Optimised for SEO (it has been a standard)

**Cons of server-side routing :**
- Full page refresh
- Can be slow if very large dataset

A client-side route happens when the route is handled internally by the JavaScript that is loaded on the page. The URL changes but the request to the server is prevented.

It is important to note that the whole page won’t refresh when using client-side routing. There are just some elements inside the application that will change.

**Pros of client-side routing :**
- Whole web app needs to be loaded on te first request : initial loading time is slow
- Extra library
- SEO less optimized

**Cons of client-side routing :**
- Full page refresh
- Can be slow if very large dataset

## Rendering charts

- Regular JS :

<img src="chart-ssr-1.png" width="500px"/>

- Server side rendering :

<img src="chart-ssr-2.png" width="500px"/>

- Progressive rendering :

<img src="chart-ssr-3.png" width="500px"/>

## Resources

- [Client-side rendering vs Server-side rendering](https://medium.freecodecamp.org/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d)
- [Server-side vs client-side routing](https://medium.com/@wilbo/server-side-vs-client-side-routing-71d710e9227f) 
- [Now](https://zeit.co/now)