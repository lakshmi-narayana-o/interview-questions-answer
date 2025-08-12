# 🚀 Practical Next.js Interview Q&A

### 1\. What's the difference between SSR, SSG, and CSR? When would you use each?

#### **Simple Answer**

*   **SSG (Static Site Generation):** Builds the page once at build time. Super fast, like a pre-made meal. 🚀
    
*   **SSR (Server-Side Rendering):** Builds the page on the server for every single request. Always fresh, like a meal cooked to order. 👨‍🍳
    
*   **CSR (Client-Side Rendering):** Your browser gets a nearly empty page and then uses JavaScript to build the content. It's like getting a DIY meal kit. 📦
    

#### **Long Explanation**

These three are different strategies for rendering your web pages, each with its own trade-offs.

*   **Static Site Generation (SSG):** With SSG, the HTML for a page is generated at **build time**—that is, when you run next build. This pre-built HTML file is then hosted on a CDN and can be served to users instantly.
    
    *   **Pros:** Extremely fast performance (TTFB - Time To First Byte is very low), secure, and great for SEO since search engine crawlers receive a fully rendered HTML page.
        
    *   **Cons:** The content is static. If the data changes, you need to rebuild and redeploy your entire site. This can be mitigated with **Incremental Static Regeneration (ISR)**, which allows you to rebuild specific pages on a timer or on-demand.
        
    *   **Use Cases:** Blogs, marketing websites, documentation, portfolios—any content that doesn't change frequently.
        
    *   **Next.js Implementation:** Using getStaticProps in the Pages Router or the default behavior of fetch in the App Router.
        
*   **Server-Side Rendering (SSR):** With SSR, the HTML for a page is generated on the server **for each incoming request**. The server fetches the necessary data, renders the React components into an HTML string, and sends that fully populated page to the browser.
    
    *   **Pros:** The data is always up-to-date. It's also excellent for SEO for the same reason as SSG.
        
    *   **Cons:** It's slower than SSG because a page has to be generated on every request, which increases server load and TTFB.
        
    *   **Use Cases:** E-commerce sites (with changing prices/stock), user dashboards, news feeds—any page that needs to display real-time, dynamic data.
        
    *   **Next.js Implementation:** Using getServerSideProps in the Pages Router or opting out of caching (cache: 'no-store') in the App Router.
        
*   **Client-Side Rendering (CSR):** With CSR, the server sends a minimal HTML file with a JavaScript bundle. The browser then executes the JavaScript, which fetches data from an API and renders the content into the DOM. This is the traditional way Single Page Applications (SPAs) like those made with plain React work.
    
    *   **Pros:** After the initial load, navigation between pages can feel very fast since you're just fetching data, not entire HTML pages. Great for highly interactive web applications.
        
    *   **Cons:** Poor for SEO because crawlers might see a blank page before the JavaScript runs. The initial page load can be slow for the user as they have to wait for the JS to download, parse, and execute.
        
    *   **Use Cases:** Heavily interactive applications where SEO isn't a primary concern, like an internal admin panel or a complex tool like Figma.
        
    *   **Next.js Implementation:** Using a combination of useEffect and useState in a component to fetch data after it mounts.

**Example:**
```jsx
// SSG (Pages Router) - Page is pre-rendered at build time
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();
  return { props: { posts } };
}

// SSR (Pages Router) - Page is rendered on every request
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/products');
  const products = await res.json();
  return { props: { products } };
}

// CSR (in any component) - Data is fetched in the browser
import { useState, useEffect } from 'react';

function Profile() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('/api/user')
      .then((res) => res.json())
      .then((data) => {
        setUser(data);
        setLoading(false);
      });
  }, []);

  if (loading) return <p>Loading...</p>;
  return <h1>Welcome, {user.name}</h1>;
}
```
### 2\. Explain the App Router vs. the Pages Router in Next.js. What are the key advantages of the App Router?

#### **Simple Answer**

The **Pages Router** is the original way Next.js handled routing, where every file in the pages directory is a route. The new **App Router** uses folders and special files (like page.js and layout.js) inside an app directory. Its main advantages are **React Server Components**, **layouts that don't re-render**, and better support for streaming and nested routes.

#### **Long Explanation**

The introduction of the App Router in Next.js 13 was a paradigm shift.

*   **Pages Router:**
    
    *   **Structure:** A file like pages/about.js creates the /about route. A file like pages/posts/\[id\].js creates dynamic routes like /posts/123.
        
    *   **Data Fetching:** Relies on specific functions exported from the page file: getStaticProps (for SSG) and getServerSideProps (for SSR).
        
*   **App Router:**
    
    *   **Structure:** Routing is now directory-based. The /dashboard route is created by a folder named dashboard containing a page.js file (app/dashboard/page.js). This structure makes it easier to co-locate related files like components, styles, and tests.
        
    *   **Special Files:** It uses a convention of special file names:
        
        *   page.js: The main UI for a route.
            
        *   layout.js: A shared UI that wraps child pages and layouts. **Crucially, layouts preserve state and don't re-render on navigation.**
            
        *   loading.js: Automatically creates a loading UI using React Suspense.
            
        *   error.js: Automatically creates an error boundary to catch errors.
            
    *   **Key Advantages:**
        
        1.  **React Server Components (RSC):** By default, all components in the App Router are Server Components. They render only on the server, resulting in zero client-side JavaScript for those parts, which significantly improves performance. You can opt-in to interactivity with Client Components using the "use client" directive.
            
        2.  **Shared & Nested Layouts:** You can easily create complex nested layouts that persist across route changes, avoiding unnecessary re-renders and preserving component state (like scroll position or input fields).
            
        3.  **Simplified Data Fetching:** Data fetching is now built into the native fetch API, which is automatically deduped and cached. You no longer need getStaticProps or getServerSideProps.
            
        4.  **Streaming:** The App Router supports streaming responses with loading.js and React Suspense, allowing you to show parts of the page instantly while the rest of the data is still being fetched. This improves the perceived performance.
            

#### **Example Code Snippets**

**File Structure Comparison**
```
# Pages Router
pages/
├── _app.js
├── index.js
└── dashboard/
    └── settings.js   # Corresponds to /dashboard/settings

# App Router
app/
├── layout.js         # Root layout
├── page.js           # Home page
└── dashboard/
    ├── layout.js     # Dashboard-specific layout
    └── settings/
        └── page.js   # Corresponds to /dashboard/settings
```

***App Router Layout Example (app/dashboard/layout.js)***
```js
// This layout wraps all pages inside the /dashboard route
export default function DashboardLayout({ children }) {
  return (
    <section>
      {/* Include a shared dashboard sidebar or header here */}
      <nav>Dashboard Nav</nav>
      {/* The actual page content will be rendered here */}
      {children}
    </section>
  );
}
```
