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
- very confusing but nextjs lets us create api routes
  - pages/api/hello.js
  - eg 
```
export default function handler(req, res) {
  res.status(200).json({ text: 'Hello' });
}
```
- [wat](https://nextjs.org/learn/basics/api-routes/api-routes-details)
- sheesh

### deploying your nextjs app
- [workflow reccs](https://nextjs.org/learn/basics/deploying-nextjs-app/platform-details)
- other hosting options
```
// package.json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start",
    // if specific port
    "start": "next start -p $PORT",
  }
}
```
- [what to learn next](https://nextjs.org/learn/basics/deploying-nextjs-app/finally)

### seo notes
- seo gud cuz
  - qualitative, more visitors = more customers
  - trustable, higher confidence in your brand or mission
  - low cost, aside from time and effort, good seo practices = higher search ranking = free top organic search
- 3 PILLARS OF OPTIMIZATION
  - technical, optimize website for crawlking and web performance
  - creation, create content stratergy to target specific words
  - popularity, boost presence online so search engines know me trusted source, backlinks = good, thirdparty site that link back to me side = backlink
- search systems
  - crawling, process of going through the web and parsing contents of all websites
  - indexing, finding place to store data gathered from crawling
  - rendering, executing js on pages that might enhance features and enrich content on the site. rendering might happen after indexing cuz no resources on their end to render task at that time
  - ranking. querying data to show relevant result based on user input, this is where different ranking criteria are applied to search engines to give users the best ansewr to fufill their intent
- whot web crawlers
  - different search engines have different market shares in each country
    - google
    - bing
    - yandex
    - baidu
    - naver
    - yahoo
    - duckduckgo
  - web crawlers identify themselves using custom [user-agents](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)
  - google has [several webcrawlers](https://developers.google.com/search/docs/advanced/crawling/overview-google-crawlers)
  - how does google bot works?
    - ![google bot crawling process](https://nextjs.org/_next/image?url=%2Fstatic%2Fimages%2Flearn%2Fseo%2Fgooglebot.png&w=1920&q=75)
      1. find urls
        - using [google search console](https://search.google.com/search-console)
        - links between sites
        - [xml sitemaps](https://developers.google.com/search/docs/advanced/sitemaps/overview)
      2. http request
        - `200` - crawls and parses html
        - `30X` - it follws the redirects
        - `40X` - it will note the error ant not load the HTML
        - `50X` - it may come back after if status code has changed
          - status code, 
      3. render queue
         - a queue for indexing sites with js, but its uses mroe resources thus urls rendered are a smaller percentage over total pages out there on the internet
      4. ready to be indexed
         - if all criteria are ment, pages may be eligible to be indexed and be shown in search results. 
      5. futher reading
         1. Google: [SEA starter guide](https://developers.google.com/search/docs/beginner/seo-starter-guide)
         2. MDN: [MDN: User-Agents](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/User-Agent)