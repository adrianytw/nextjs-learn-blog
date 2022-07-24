This is a starter template for [Learn Next.js](https://nextjs.org/learn).

## notes
### navigate between pages
- import Link from `'next/link'`
- auto optimize by
- code splitting
- client-side navigation
- prefetching (in production)
- for more info
- https://nextjs.org/docs/api-reference/next/link
- https://nextjs.org/docs/routing/introduction
- eg.
    - https://github.com/vercel/next-learn/blob/master/basics/snippets/link-classname-example.js
```
<Link href='posts/first-post'/>
        <a>text for link<a/>
</Link>
```

### assets, metadata, and css
- [css docs](https://nextjs.org/docs/basic-features/built-in-css-support)
- [head api docs](https://nextjs.org/docs/api-reference/next/head)
- [if want custom html tag](https://nextjs.org/docs/advanced-features/custom-document)
- make component file at top level
- make `layout.js`
- then make `layout.module.css`
```
// for images
import Image from 'next/image';

const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
);
```
```
// for module css
import styles from './layout.module.css';

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>;
}
```
- global css
  - make `pages/_app.js` file
  - at top level dir make styles
    - then `global.css` // this for global
- polished layout
  - make `utils.module.css` at styles/
  - updated some files to make it pretty
- tips
  - classnames library to easily change classnames for css
  - 
```
// global style
import '../styles/global.css';

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```
```
// alert.module.css
.success {
  color: green;
}
.error {
  color: red;
}

// the component
import styles from './alert.module.css';
import cn from 'classnames';

export default function Alert({ children, type }) {
  return (
    <div
      className={cn({
        [styles.success]: type === 'success',
        [styles.error]: type === 'error',
      })}
    >
      {children}
    </div>
  );
}
```

### pre-rendering and data-fetching
- npm install gray-matter
  - for reading metadata on md
  - getStaticProps for static site generation with data
```
// examples of fetching data
export async function getStaticProps() {
  const allPostsData = getSortedPostsData();
  return {
    props: {
      allPostsData,
    },
  };
}
// fetch external api
export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..');
  return res.json();
}
// query database
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```
- in development `npm run dev` or `yarn dev`, getStaticProps runs on every request
- in production, `getStaticProps` runs at build time, this behaviour can be enhanced using `fallback` key returned by `getStaticPaths`
  - `getStaticProps` can only be exported from a page cuz react needs all data before render
- what if need fetch data at request time?
  - ssg no good if predender required ahead of user's request
  - use ssr or no pre-render instead
- `getServerSideProps` example
  - and the result cannot be cached by a CDN without extra configuration.
```
// for server side rendering
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    },
  };
}
```
- client-side rendering
  - nextjs recommends using [SWR](https://swr.vercel.app/),
    - it handles
      - caching
      - revalidation
      - focus tracking
      - refetching on interval
      - and more.
```
// example of swr
import useSWR from 'swr';

function Profile() {
  const { data, error } = useSWR('/api/user', fetch);

  if (error) return <div>failed to load</div>;
  if (!data) return <div>loading...</div>;
  return <div>hello {data.name}!</div>;
}
```

### dynamic routes
- dynamic routes
  - generates routes/url from variable to a link
    - e.g. /posts/[id]
  - `[var].js` files are dynamic routes in nextjs 
  - ![summary for dem routes](https://nextjs.org/static/images/learn/dynamic-routes/how-to-dynamic-routes.png)
  - [link cuz i know i will come back here](https://nextjs.org/learn/basics/dynamic-routes/implement-getstaticprops)
- render markdown
  - npm install remark remark-html
- Catch-all Routes
  - `pages/posts/[...id].js` matches `/posts/a`, but also `/posts/a/b`, `/posts/a/b/c` and so on.
- Router
  - If you want to access the Next.js router, you can do so by importing the [`useRouter`](https://nextjs.org/docs/api-reference/next/router#userouter) hook from [`next/router`](https://nextjs.org/docs/api-reference/next/router).
- [404 pages](https://nextjs.org/docs/advanced-features/custom-error-page#404-page)
```
// pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>;
}
```
- [bruh.](https://nextjs.org/learn/basics/dynamic-routes/dynamic-routes-details)

### api routes