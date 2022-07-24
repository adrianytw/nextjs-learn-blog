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

## seo notes
### introduction to seo
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

### crawling and indexing
- http status codes?
  - many exist but only few matter to seo
  - 200
    - needed for seo cuz it stants for success status
    - aka `HTTP 200 ok`
  - 301/308
    - this is a permanent redirect
    - `HTTP 301 Moved Permanently`
    - redirects are needed to ensure if content moved, you do not lose current and potential clients and bots can continue to index me site
    - nextjs use 308 by default because its newer and considered the better option
```
//  pages/about.js
export async function getStaticProps(context) {
  return {
    redirect: {
      destination: '/',
      permanent: true, // triggers 308
    },
  };
}

// OR

//next.config.js

module.exports = {
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true, // triggers 308
      },
    ];
  },
};
```
  - 302
    - `HTTP 302 Found`
    - temp moved to another location
  - 404
    - indicates that server cant find the requested source
    - `HTTP 404 Not Found`
    - not necessarily bad
      - as it is desired outcome when user happens to visit a url that dosent exist 
      - but shouldnt be frequent error as it could lead to lackluster search rankings
    - in some cases, we want to trigger 404, + custon 404 page
```
export async function getStaticProps(context) {
  return {
    notFound: true, // triggers 404
  };
}
// pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>;
}
```
  - 410
    - `HTTP 410 Gone`
    - client error responses indicates access to target resource is no longer available at the origin server and this condition is likely permanent
      - examples where a 410 gone might be wise to include 
        - e-commerce: product permanently removed from inventory
        - forum: threads that have been deleted by the user
        - blog: blog posts removed from site
    - status code informs bots that they should never return to crawl this content
  - 500
    - `HTTP 500 Internal Server Error`
    - server encounted unexpected condition that prevented it from fullfilling the request
```
eg
// pages/500.js
export default function Custom500() {
  return <h1>500 - Server-side error occurred</h1>;
}
```
  - 503
    - `HTTP 503 Service Unavailable`
    - indicates server not ready to handle request
    - recc to return this code when website is down
    - and i predict that the website will be down by an extended period of time.

- robots.txt
  - file tells search engine crawlers which pages or files to crawler can or can't request from your site.
  - file is a web standard file that good bots consume before requesting anyhthing for a specific domain
  - to add one in next js
    - add `public/robots.txt`
  ```
  //robots.txt

  # Block all crawlers for /accounts
  User-agent: *
  Disallow: /accounts

  # Allow all crawlers
  User-agent: *
  Allow: /
  ```
  - further reading
    - Google: [Create and Submit a `robot.txt` file](https://developers.google.com/search/docs/advanced/robots/create-robots-txt)

- xml sitemaps
  - easiest way to communicate with google
  - they indicate urls that belong to your website and when they update
  - google can easily detect new content and crawl me website more efficiently
  - even though xml sitemaps are most know and used ones,
  - they can also be created via
    - [rss](https://developers.google.com/search/docs/advanced/sitemaps/build-sitemap)
    - [atom](https://developers.google.com/search/docs/advanced/sitemaps/build-sitemap)
    - [text](https://developers.google.com/search/docs/advanced/sitemaps/build-sitemap) files for maximum simplicity
  - might need sitemaps if 
    - site is very large
      - google might overlook some new or recently updated pages
  - site has large archive of content that are isolated or not well linked to each other
    - if site pages dont naturally reference each other, you can list them in a sitemap to ensure that google dosent overlook some pages
  - site is new and has new external links to it
    - googlebot and other web crawlers navigate web by following links from page to page. google might not discover pages if no sites link to mine
  - site has a lot of rich media content or is shown in google news
    - if provided, google can take additional informatino from sitemaps into account for search where appropriate
  - sitemaps not mandatory for great search engine performance,
  - but they can facilitate crawling and indexing will be picked faster.
  - its strongly recommended to use sitemaps and make them dynamic.
  - ways to make sitemaps
```
<!-- public/sitemap.xml -->
   <xml version="1.0" encoding="UTF-8">
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <url>
       <loc>http://www.example.com/foo</loc>
       <lastmod>2021-06-01</lastmod>
     </url>
   </urlset>
   </xml>

// or getServerSideProps
//pages/sitemap.xml.js
const EXTERNAL_DATA_URL = 'https://jsonplaceholder.typicode.com/posts';

function generateSiteMap(posts) {
  return `<?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <!--We manually set the two URLs we know already-->
     <url>
       <loc>https://jsonplaceholder.typicode.com</loc>
     </url>
     <url>
       <loc>https://jsonplaceholder.typicode.com/guide</loc>
     </url>
     ${posts
       .map(({ id }) => {
         return `
       <url>
           <loc>${`${EXTERNAL_DATA_URL}/${id}`}</loc>
       </url>
     `;
       })
       .join('')}
   </urlset>
 `;
}

function SiteMap() {
  // getServerSideProps will do the heavy lifting
}

export async function getServerSideProps({ res }) {
  // We make an API call to gather the URLs for our site
  const request = await fetch(EXTERNAL_DATA_URL);
  const posts = await request.json();

  // We generate the XML sitemap with the posts data
  const sitemap = generateSiteMap(posts);

  res.setHeader('Content-Type', 'text/xml');
  // we send the XML to the browser
  res.write(sitemap);
  res.end();

  return {
    props: {},
  };
}

export default SiteMap;
```

- special meta tags for search engines
  - they belong in head tag
  - meta robot tags
  - `<meta name="robots" content="noindex,nofollow" />`
  - `<meta name="robots" content="all" />`
    - noindex
      - to not show this page in search results
    - nofollow
      - to not follow links on this page
    - more [info](https://developers.google.com/search/docs/advanced/robots/robots_meta_tag#directives)
  - google tags
    - nositelinkssearchbox
      - `<meta name="google" content="nositelinkssearchbox" />`
    - notranslate
      - `<meta name="google" content="notranslate" />`

-  canonical tags?
   - canonical url is a url of that page that most representative from a set of duplicate pages on your site
   - if google thinks multiple pages is the same, the rank might go down cuz duplicated
   - canonical tag lets google know the original source of thruth and which is duplicated.
```
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Canonical Tag Example</title>
        <link
          rel="canonical"
          href="https://example.com/blog/original-post"
          key="canonical"
        />
      </Head>
      <p>This post exists on two URLs.</p>
    </div>
  );
}

export default IndexPage;
```
   - further reading
     - google: [consolidate duplicate urls](https://developers.google.com/search/docs/advanced/crawling/consolidate-duplicate-urls)
     - next.js [i18n Routing](https://nextjs.org/docs/advanced-features/i18n-routing)

### rendering and ranking
- render strats
  - static site generation, ssg
    - rendered at build time, serves static
  - server-side generation, ssr
    - rendered upon request, serves static
  - incremental static regeneration, isr
    - allows for render for per-page basis, without needing to rebuild the entire site.
      - retain static benefits while scaling to millions of pages
  - client side rendering, csr
    - served with empty html and css, rendered at clients side
    - not recommended for optimal seo
  - further reading
    - Nextjs: [data fetching](https://nextjs.org/docs/basic-features/data-fetching)
    - smashing magazine: [a complete guide o incremental staic generaltion with nextjs](https://www.smashingmagazine.com/2021/04/incremental-static-regeneration-nextjs)
    - vercel: [nextjs: ssr vs ssg](https://vercel.com/blog/nextjs-server-side-rendering-vs-static-generation)
- [amp](https://amp.dev/)
  - enables devs to create web pages that load faster on mobile pages at the cost of building and maintaining them.
  - with [core web vitals](https://web.dev/vitals/), google dropped amp pages requirement to appear in search carousels
  - nextjs offers amp support, but consider weighing the costs and benefits of having amp implementation if me site has great core web vital scores.
- url structure
  - it is important part of an seo stratergy.
  - google dosent disclose which weight each part of seo has,
  - but good urls are considered best practice
  - principles
    - sementic
      - they use words instead of random numbers
      - e.g.
        - ` /learn/basics/create-nextjs-app` better than `/learn/course-1/lesson-1`
    - patterns that are logical and consistent
      - consistency
      - url should follow some sort of patern.
      - instead of having different paths for each product
    - keyword focused
      - google still bases a considerable part of their systems on the keywords a website contains
      - might want to use keywords in url to facilitate understanding the purpose of the pages.
    - not parameter-based
      -  using parameters to build urls is no gud
      -  they are not semantic in most cases
      -  search engine might get confused and demote rankings
 -  how routes are defined in nextjs?
    -  simple urls
    -  Homepage: https://www.example.com → pages/index.js
    - Listings: https://www.example.com/products → pages/products.js or pages/products/index.js
    - Detail: https://www.example.com/products/product → pages/products/product.js
  - for blogs or ecommerce site, me likely want to use product id or blog name as slug for the url, 
    - aka dynamic routing
    - Product:https://www.example.com/products/nextjs-shirt → pages/products/[product].js
    - Blog:https://www.example.com/blog/seo-in-nextjs → pages/blog/[blog-name].js
  - example of seo optimized pages
for ssg
```
// pages/blog/[slug].js

import Head from 'next/head';

export async function getStaticPaths() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://www.example.com/api/posts');
  const posts = await res.json();

  // Get the paths we want to pre-render based on posts
  const paths = posts.map((post) => ({
    params: { slug: post.slug },
  }));
  // Set fallback to blocking. Now any new post added post build will SSR
  // to ensure SEO. It will then be static for all subsequent requests
  return { paths, fallback: 'blocking' };
}

export async function getStaticProps({ params }) {
  const res = await fetch(`https://www.example.com/api/blog/${params.slug}`);
  const data = await res.json();

  return {
    props: {
      blog: data,
    },
  };
}

function BlogPost({ blog }) {
  return (
    <>
      <Head>
        <title>{blog.title} | My Site</title>
      </Head>
      <div>
        <h1>{blog.title}</h1>
        <p>{blog.text}</p>
      </div>
    </>
  );
}

export default BlogPost;
```
another for ssr
```
// pages/blog/[slug].js

import Head from 'next/head';
function BlogPost({ blog }) {
  return (
    <div>
      <Head>
        <title>{blog.title} | My Site</title>
      </Head>
      <div>
        <h1>{blog.title}</h1>
        <p>{blog.text}</p>
      </div>
    </div>
  );
}

export async function getServerSideProps({ query }) {
  const res = await fetch(`https://www.example.com/api/blog/${query.slug}`);
  const data = await res.json();

  return {
    props: {
      blog: data,
    },
  };
}

export default BlogPost;
```
  - further reading
    - Next.js: [Introduction to Routing](https://nextjs.org/docs/routing/introduction)
    - Next.js: [Pages](https://nextjs.org/docs/basic-features/pages)
    - Next.js: [Dynamic Routing](https://nextjs.org/docs/routing/dynamic-routes)

- metadata, the abstract of websites content, used to attatch a title. a description, and an image to the site.
  - title
    - title is one of the most important
      - what user sees when they click to enter site
      - main elements google uses to understand what me page is about
        - using keywords in title is guuud
        - eg `<title>iPhone 12 XS Max For Sale in Colorado - Big Discounts | Apple</title>`
  - description
    - another important but less so than title
      - not taken into account for ranking purposes, but affect click through rate.
    - use description meta tag to complement the information in title.
    - work in more keywords here if some didnt fit the title.
    - keywords appear in bold if users search contain them
    - eg
```
<meta
  name="description"
  content="Check out iPhone 12 XR Pro and iPhone 12 Pro Max. Visit your local store and for expert advice."
/>
```
```
// example of whole
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>
          iPhone 12 XS Max For Sale in Colorado - Big Discounts | Apple
        </title>
        <meta
          name="description"
          content="Check out iPhone 12 XR Pro and iPhone 12 Pro Max. Visit your local store and for expert advice."
          key="desc"
        />
      </Head>
      <h1>iPhones for Sale</h1>
      <p>insert a list of iPhones for sale.</p>
    </div>
  );
}

export default IndexPage;
```
  - open graph
    - the [open graph protocol](https://ogp.me/)
    - standarizes metadata is used on any given webpage
    - good for sharing on social media sites.
    - dosent really affect seo but good idea for sharing (backlinks).
```
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Cool Title</title>
        <meta name="description" content="Checkout our cool page" key="desc" />
        <meta property="og:title" content="Social Title for Cool Page" />
        <meta
          property="og:description"
          content="And a social description for our cool page"
        />
        <meta
          property="og:image"
          content="https://example.com/images/cool-page.jpg"
        />
      </Head>
      <h1>Cool Page</h1>
      <p>This is a cool page. It has lots of cool content!</p>
    </div>
  );
}

export default IndexPage;
```
  - structured data and json-ld
    - facilitates understanding of your pages to search engines
      - many over the years but [schema.org](https://schema.org/) main one
```
// eg
import Head from 'next/head';

function ProductPage() {
  function addProductJsonLd() {
    return {
      __html: `{
      "@context": "https://schema.org/",
      "@type": "Product",
      "name": "Executive Anvil",
      "image": [
        "https://example.com/photos/1x1/photo.jpg",
        "https://example.com/photos/4x3/photo.jpg",
        "https://example.com/photos/16x9/photo.jpg"
       ],
      "description": "Sleeker than ACME's Classic Anvil, the Executive Anvil is perfect for the business traveler looking for something to drop from a height.",
      "sku": "0446310786",
      "mpn": "925872",
      "brand": {
        "@type": "Brand",
        "name": "ACME"
      },
      "review": {
        "@type": "Review",
        "reviewRating": {
          "@type": "Rating",
          "ratingValue": "4",
          "bestRating": "5"
        },
        "author": {
          "@type": "Person",
          "name": "Fred Benson"
        }
      },
      "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.4",
        "reviewCount": "89"
      },
      "offers": {
        "@type": "Offer",
        "url": "https://example.com/anvil",
        "priceCurrency": "USD",
        "price": "119.99",
        "priceValidUntil": "2020-11-20",
        "itemCondition": "https://schema.org/UsedCondition",
        "availability": "https://schema.org/InStock"
      }
    }
  `,
    };
  }
  return (
    <div>
      <Head>
        <title>My Product</title>
        <meta
          name="description"
          content="Super product with free shipping."
          key="desc"
        />
        <script
          type="application/ld+json"
          dangerouslySetInnerHTML={addProductJsonLd()}
          key="product-jsonld"
        />
      </Head>
      <h1>My Product</h1>
      <p>Super product for sale.</p>
    </div>
  );
}

export default ProductPage;
// In this example, the data is hardcoded as a string, but you can easily pass the data to the `addProductJsonLd` method to make it fully dynamic.
```
  - further reading
    - Open Graph Protocol: [Documentation](https://ogp.me/)
    - Google: [Intro to Structured Data](https://developers.google.com/search/docs/guides/intro-structured-data)
    - Google: [Product Structured Data](https://developers.google.com/search/docs/data-types/product)
    - Google Search: [Rich Results Tester](https://search.google.com/test/rich-results)

- on page seo
  - at high level, it refers to headings and links that make up the overall structure of the page
  - headings indicate importance in the document and links connect the web together
  - headings and h1
    - headings help users understand structure of the page and what they are going to read in the next paragraphs.
    - they also facilitate search engines job of understanding which parts of the page are most important
    - heading 1-6 tends to be thought as the most important
    - reccomended to use h1 on each page and represents what the page is about
  - internal links
    - websites that receive more links tend to represent sites that are more trusted by users
    - [pagerank algorithm](https://web.stanford.edu/class/cs54n/handouts/24-GooglePageRankAlgorithm.pdf)
      - algo that goes through every link on a db and scores domains based on how many links they receive(quantity) and from which domains(quality), lots of links from spam sites most likely have little to no value
      - link from national press websites is likely valueble for search engines
      - important to always include both interally between page and externally to other sites
      - use `href` to be used for pagerank calcs
    - next/link
      - href prop is required and will correctly add link to anchor tag
      - however, if child of link is a custom component that wraps an `a` tag, i must add `passHref` to `link`
      - necessary for libs like styled components
```
// how to use passHref

import Link from 'next/link';
import styled from 'styled-components';

// This creates a custom component that wraps an <a> tag
const RedLink = styled.a`
  color: red;
`;

function NavLink({ href, name }) {
  // Must add passHref to Link
  return (
    <Link href={href} passHref>
      <RedLink>{name}</RedLink>
    </Link>
  );
}

export default NavLink;
```
  - further reading
    - [next/link documentation](https://nextjs.org/docs/api-reference/next/link)
    - [eslint-plugin-next documentation](https://nextjs.org/docs/basic-features/eslint)