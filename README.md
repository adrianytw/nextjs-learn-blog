This is a starter template for [Learn Next.js](https://nextjs.org/learn).

## notes
### navigate between pages
- import Link from 'next/link'
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
- make layout.js
- then make layout.module.css
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
    - then global.css // this for global
- polished layout
  - make utils.module.css at styles/
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