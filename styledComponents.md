# Next.js

## [Setup](https://nextjs.org/docs)



1. create `package.json` with `npm init`
1. install: `npm install --save next react react-dom`
1. Add script to `package.json`:
    ```js
    {
    "scripts": {
        "dev": "next -p <port number>",
        "build": "next build",
        "start": "next start"
        }
    }
    ```
1. create pages/index.js and populate with

    ```js
    function Home() {
        return <div>Welcome to Next.js!</div>;
    }

    export default Home;
    ```

1. start dev server with `npm run dev`

## Optional

### Installing styled-components

1. `npm install --save styled-components`
1. Install Babel plugin `npm install --save-dev babel-plugin-styled-components`
1. create a .babelrc file and add:

    ```js
    {
        "presets": ["next/babel"],
        "plugins": [
            [
                "styled-components",
                {
                    "ssr": true,
                    "displayName": true,
                    "preprocess": false
                }
            ]
        ]
    }
    ```
1. Add a custom`_document.js` to the `pages` directory

1. Add SSR styled components
    ```js
    import Document, { Head, Main, NextScript } from 'next/document';
    // Import styled components ServerStyleSheet
    import { ServerStyleSheet } from 'styled-components';

    export default class MyDocument extends Document {
    static getInitialProps({ renderPage }) {
        // Step 1: Create an instance of ServerStyleSheet
        const sheet = new ServerStyleSheet();

        // Step 2: Retrieve styles from components in the page
        const page = renderPage((App) => (props) =>
        sheet.collectStyles(<App {...props} />),
        );

        // Step 3: Extract the styles as <style> tags
        const styleTags = sheet.getStyleElement();

        // Step 4: Pass styleTags as a prop
        return { ...page, styleTags };
    }

    render() {
        return (
        <html>
            <Head>
            <title>My page</title>
            {/* Step 5: Output the styles in the head  */}
            {this.props.styleTags}
            </Head>
            <body>
            <Main />
            <NextScript />
            </body>
        </html>
        );
    }
    }
    ```
## Adding pages

Next takes care of all the routing between pages out of the box, the only thing you need to do to support client-side navigation (no reload) is to import `Link from 'next/link`, then you wrap the anchor tag and pass the route to the `href`in `Link`:

```js
import Link from 'next/link'

const Index = () => (
  <div>
    <Link href="/about">
      <a  >About Page</a>
    </Link>
    <p>Hello Next.js</p>
  </div>
)
export default Index
```

`Link` is a HOC that only accepts href and a limited amount of props, if you need to add props or attributes, pass them directly to the underlying component (it can be any element, not just `a`, the only requirerment is that they accept an `onClick` prop):

```diff
-<Link href="/about" title="About Page">
-      <a >About Page</a>
-</Link>

+<Link href="/about">
+      <a  title="About Page">About Page</a>
+</Link>
```

[Next.js rounting docs](https://nextjs.org/docs/#routing)


