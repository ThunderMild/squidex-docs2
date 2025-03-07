---
description: >-
  How to build a Next.js blog powered by content from Squidex CMS.
---

# Building a blog with Squidex and Next.js

### Introduction

This tutorial will guide you to create a blog application, set up its schema, add mock content on Squidex, and create a Next.js blog based on the data stored in your CMS.

## Prerequisites

To complete this guide, you need:

- A Squidex account. You can create one at https://cloud.squidex.io
- Node.js installed on your computer. Follow this guide to install [Node.js](https://phoenixnap.com/kb/install-node-js-npm-on-windows) if you haven't already.

## Step 1: Creating the app on Squidex

In Squidex, an application is an isolated data store with schemas that define how its content is structured and API endpoints to query its content. You can visit [this page](https://docs.squidex.io/02-documentation/concepts/apps) of the Squidex docs to learn more about apps in Squidex.

Start by logging into your Squidex account dashboard at https://cloud.squidex.io/. Click _New App_ to create an empty application. Enter a name you choose for the app in the space provided and click _Create_ to create the app. For the sake of this guide, we will assume the name of your Squidex application is `squidex-blog`.

## Step 2: Creating the posts schema on the CMS

Now that you have created the app on Squidex, you will proceed to create the schema for the blog application. This schema will define the structure of your content.

Still on your Squidex account dashboard at https://cloud.squidex.io, click _squidex-blog_ (or whatever name you chose) on the dashboard to enter the dashboard for your newly created app.

On the left panel, select _Schemas_ and click the Plus(_+_) button to start creating the posts schema. Enter `posts` in the space provided under _Name_, select _Multiple contents_ since you will be having many posts, and click _Create_ to create the schema.

You will be creating three fields under the posts schema: `Title` for the title of your posts, `Slug` to define the URL of your posts, and `Content` to contain the body of your blog posts.

| Field name | Type   | Editor   |
| ---------- | ------ | -------- |
| Title      | String | Input    |
| Slug       | String | Slug     |
| Content    | String | Markdown |

Select _posts_ from the sidebar under _Schemas_ and click _Add Field_ to start adding fields. Configure the `Title` field in the modal window as shown below:

![Create Posts Title Field](../../../images/nextjs-blog-tutorial/adding-title-field-to-posts-annotated.jpg)

Click _Create and Edit field_ to continue.

Fill in the validation tab of the new modal that will open as shown below:

![Configuring validation for the posts Title field](../../../images/nextjs-blog-tutorial/posts-title-validation.jpg)

The `Title` field is required since every post must have a title. Click _Save and add field_ to save your changes and proceed to add the `Slug` field.

Configure the `Slug` field as shown below:

![Creating the posts Slug field](../../../images/nextjs-blog-tutorial/adding-slug-field-to-posts.jpg)

Click _Create and edit field_ to continue.

Since the every post must have a slug and that slug has to be unique, complete the _Validation_ tab in the modal that opens as shown below:

![Make the Slug field unique and required](../../../images/nextjs-blog-tutorial/make-slug-field-unique.jpg)

Switch to the _Editing_ tab and select _Slug_ as the editor as shown below:

![Use slug editor for Slug field](../../../images/nextjs-blog-tutorial/use-slug-editor-for-slug.jpg)

The options at this _Editor_ section allow you to choose what type of editor you want to use to edit a particular field. Click _Save and add field_ to save the `Slug` field and proceed to add the `Content` field.

Your blog content will be formatted as a [Markdown](https://daringfireball.net/projects/markdown/) text. Configure the `Content` field as shown below:

![Create Content field for blog post body](../../../images/nextjs-blog-tutorial/create-content-field.jpg)

Click _Create and edit field_ to continue. Set the `Content` field to use a WYSIWYG editor by configuring the _Editing_ tab as shown below:

![Use markdown editor for Content field](../../../images/nextjs-blog-tutorial/use-markdown-editor-for-content.jpg)

Click _Save and close_ to save the changes to the `Content` field.

You have now finished creating the posts schema and its fields. To be able to add content under the posts schema, you will publish the posts schema. Click _Published_ around the top right corner of the page to publish it.

Now your schema is published, you will add sample content to your schema that will be displayed once your blog is built. To add content, click _Content_ on the sidebar and select _posts_ if it is not already selected. Click _New_ to create a new post. If you have some content you want to transfer to Squidex, you can paste it here. If you don't, you can manually enter some placeholders for title, slug and content then click _Save and Publish_ to publish the content you created. Click the back icon and _New_ again to create another sample blog post. Repeat this for as many posts as you like then proceed to the next step.

## Step 3: Setting up your Next.js workspace

Now that you have created your schema on Squidex, you will proceed to set up your Next.js workspace.

Open your terminal and run the following command to install Next.js and its dependencies.

```bash
npx create-next-app squidex-blog
```

If you are prompted to confirm that you want to install the create-next-app package, type `y` and press enter to continue with the installation.

Once Next.js is set up, run the following commands to enter the `squidex-blog` directory and start the development server.

```bash
cd squidex-blog
yarn dev
```

The `yarn dev` command starts a Next.js development server at `http://localhost:3000`. Visit `htttp://localhost:3000` to see your Next.js app. You should see _Welcome to Next.js_ on the page. You have now successfully installed Next.js and are ready to start building your blog.

## Step 4: Create global styles and a Layout component

You will start by creating a `Layout` component that adds a header and footer to all the pages of your blog. Create a `components` folder in `squidex-blog` and create a `layout.js` file in the `squidex-blog/components` folder with the following contents:

```jsx
import Link from "next/link";

export default function Layout({ children }) {
  return (
    <div className="container">
      <div className="header">
        <Link href="/">
          <a className="blog-heading">My Blog</a>
        </Link>
      </div>
      {children}
      <div className="footer">
        <div>My Blog</div>
        <div>(c) 2022 </div>
      </div>
    </div>
  );
}
```

This creates a footer and a header with a link to your blog's homepage. The `children` prop allows you to embed whatever content is added into the `Layout` component between the header and footer.

After creating the `Layout` component, replace the contents of the `squidex-blog/styles/globals.css` file with the following:

```css
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 1em;
}

.header {
  margin-top: 2em;
  margin-bottom: 4em;
}

.blog-heading {
  font-size: 2em;
  font-weight: bold;
  text-decoration: none;
  color: black;
}

.footer {
  margin: 4em auto;
  width: fit-content;
}

.blog-item {
  margin-top: 20px;
}

.blog-item-title {
  font-size: 2em;
  text-decoration: none;
  color: black;
}

.blog-title {
  font-size: 3em;
}
```

Now that you have modified the website's stylesheet, you will go on to modify the custom Next.js `App` component of your blog to wrap the application in a header and footer with the `Layout` component you created. In Next.js, a custom app component is used to add a consistent layout to app pages and to add global CSS to your app. You can learn more about it from the [Custom App page of the Next.js documentation](https://nextjs.org/docs/advanced-features/custom-app).

Modify `_app.js` in `squidex-blog/pages` to have the the following content:

```jsx
import Layout from "../components/layout";
import "../styles/globals.css";

export default function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}
```

Visit `localhost:3000` if you haven't already. The development server will rebuild your blog. You will see the newly added header and footer.

Now that you have added the styles and a header and footer to your application, you will proceed to set up authentication for requests to the Squidex API.

## Step 5: Preparing to fetch data from the Squidex API

To be able to fetch data from your Squidex App, you need to create a client on Squidex.

Open the dashboard of your blog application(`squidex-blog` or whatever name you chose) from https://cloud.squidex.io. On the left sidebar, click the _cog icon_ at the bottom left corner of the page to open settings. Under the _Security_ section click _Clients_. At the top of the page, you will see a section where you can create a client. In the text box provided under _Add a new client_, type `nextjs-blog` as the name of the client and click _Add Client_ to create the client. Now when you scroll down the page you will notice a new `nextjs-blog` client has been created. By default, this new client comes with the Editor role. In Squidex, roles define which schemas a client has access to and which operations such as reading and writing a client is authorized to do. Since you wouldn't be making any changes to your content from your blog's frontend, you will change the role of the client to Reader i.e. with only read permissions. In the dropdown for _Role_, select _Reader_.

You would normally require an access token to be able to query the Squidex API, but since your blog application will only be reading data from Squidex, not editing or deleting, you will create an anonymous client. Check the _Allow anonymous access_ checkbox to allow this client to be query published blog posts without an access token.

Now you have created a new client and allowed anonymous access to published blog posts, you will create a file containing environment variables for your Next.js app. Create a `.env.local` file in the `squidex-blog` folder and add the following contents

```
SQUIDEX_API_URL=https://cloud.squidex.io/api/content/<YOUR_APP>/graphql
```

Replace `<YOUR_APP>` in the value of SQUIDEX_API_URL with the name of your app on Squidex.

Next.js only loads data from the `.env.local` file once, when you start the development server. To load the environment variables you just created, you have to restart the development server. Press the combination `Ctrl/CMD + C` on your terminal to stop the development server, then restart it by running:

```bash
yarn dev
```

You will see a message in the terminal showing that environment variables have been loaded from `.env.local`. You can visit the [Environment Variables page of the Next.js documentation](https://nextjs.org/docs/basic-features/environment-variables) to learn more about environment variables in Next.js. Now you will proceed to create a helper function that you will use to make GraphQL queries to Squidex.

Create a `lib` folder in `squidex-blog` and in this `lib` folder, create a `squidex.js` file. Add the following contents to the `squidex-blog/lib/squidex.js` file:

```js
export default async function fetchAPI(query, { variables } = {}) {
  const res = await fetch(process.env.SQUIDEX_API_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      query,
      variables,
    }),
  });
  const json = await res.json();

  if (json.errors) {
    console.error(json.errors);
    throw new Error("Failed to fetch API");
  }

  return json.data;
}
```

This exports a `fetchAPI` function that receives a GraphQL query and its variables as parameters, queries the Squidex API, and returns the result.

You are now ready to fetch content from Squidex and display it in your Next.js blog.

## Step 6: Displaying blog posts on home page

In this step, you will fetch the titles and links of your blog posts and display them on the homepage of your blog.

Replace the content of `squidex-blog/pages/index.js` with this:

```jsx
import Head from "next/head";

export default function Home() {
  return (
    <div>
      <Head>
        <title>My blog</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
    </div>
  );
}
```

Open `http://localhost:3000` in your browser, if you haven't already, to continue previewing changes.

At the top of `pages/index.js`, import the `fetchAPI` utility from `squidex-app/lib/squidex`:

```js
import fetchAPI from "../lib/squidex";
```

Also in `index.js`, export a `getStaticProps` function defined as follows:

```js
export async function getStaticProps() {
  const data = await fetchAPI(`
  {
    queryPostsContents {
      id,
      flatData {
        title,
        slug,
      }
    }
  }
  `);
  return {
    props: {
      posts: data.queryPostsContents,
    },
  };
}
```

This sends a GraphQL query to your Squidex app to get the slugs and titles of all your blog posts and returns this data to your page.

In Next.js, `getStaticProps` is a function that is run at build time to fetch and store data needed to render a page. To learn more about data fetching with getStaticProps in Next.js, see [the getStaticProps page of the Next.js documentation](https://nextjs.org/docs/basic-features/data-fetching/get-static-props). Save the file and refresh your browser to fetch page data from the CMS.

To make this data available to the `Home` page component of `index.js`, pass posts as a prop to `Home` in index.js:

```js
export default function Home({ posts }) {
...
}
```

Although the title and slugs of your blog posts are now available to `Home` component, they will not be rendered on the homepage. You will now create a `BlogItem` component that will be used to display and link to all your posts.

Create a `blogItem.js` file in the `squidex-blog/components` folder with the following content:

```jsx
import Link from "next/link";

export default function BlogItem({ title, slug }) {
  return (
    <div className="blog-item">
      <Link href={slug}>
        <a className="blog-item-title">{title}</a>
      </Link>
    </div>
  );
}
```

To use `BlogItem` on your homepage, import it in `pages/index.js`

```jsx
import BlogItem from "../components/blogItem";
```

And in the `Home` function in `index.js`, after the `Head` tag, add the following:

```jsx
<div className="blog-post-list">
  {posts.map((post) => (
    <BlogItem
      title={post.flatData.title}
      slug={post.flatData.slug}
      key={post.id}
    />
  ))}
</div>
```

This loops through your posts and creates a link to each of them.

Your completed `index.js` file will look like this:

```jsx
import Head from "next/head";
import fetchAPI from "../lib/squidex";
import BlogItem from "../components/blogItem";

export default function Home({ posts }) {
  return (
    <div>
      <Head>
        <title>My blog</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <div className="blog-post-list">
        {posts.map((post) => (
          <BlogItem
            title={post.flatData.title}
            slug={post.flatData.slug}
            key={post.id}
          />
        ))}
      </div>
    </div>
  );
}

export async function getStaticProps() {
  const data = await fetchAPI(`
  {
    queryPostsContents {
      id,
      flatData {
        title,
        slug,
      }
    }
  }
  `);
  return {
    props: {
      posts: data.queryPostsContents,
    },
  };
}
```

If you look at your blog homepage in a browser, you will see that the titles of your posts are shown and they link to the page where the article can be read.

Now that you have successfully created links to your posts on the homepage, you will create a page to show the contents of the blog posts.

## Step 7: Creating the blog post pages

In the `pages` folder, create a `[slug].js` file. In Next.js, a page is a React Component file in the pages directory. The component at pages/about.js, for example will be accessible at /about. A page enclosed in square brackets like [slug].js is a fallback and Next.js will show this page for all URLs that are not handled by another page, and will pass a `slug` variable to the page. So when you visit a URL like /my-first-blog-post this page is used and `slug` will have the value my-first-blog-post.

You can learn more about this from the [dynamic routes page of the Next.js documentation.](https://nextjs.org/docs/routing/dynamic-routes).

```jsx
export default function BlogPost() {
  return <div>Body</div>;
}
```

Click one of the links on the homepage to visit the page where your blog posts will be rendered.

Since you will need to render markdown content on this page, you will install [next-mdx-remote](https://github.com/hashicorp/next-mdx-remote), a package that parses markdown content and renders it in your Next.js application. Stop the development server by pressing `Ctrl/Cmd + C` on your terminal and run the following command to install `next-mdx-remote`:

```bash
yarn add next-mdx-remote
```

Restart the development server after installation:

```
yarn dev
```

Import `serialize` and `MDXRemote` from `next-mdx-remote` at the top of `[slug].js`:

```jsx
import { serialize } from "next-mdx-remote/serialize";
import { MDXRemote } from "next-mdx-remote";
```

Import `fetchAPI` at the top of `[slug].js` as you will use it to fetch the contents your blog posts from Squidex:

```js
import fetchAPI from "../lib/squidex";
```

Export a `getStaticProps` function from `[slug].js`:

```js
export async function getStaticProps({ params }) {
  const data = await fetchAPI(
    `
  query QueryPosts($query: String!) {
    queryPostsContents(filter: $query) {
      id
      flatData {
        title
        content
      }
    }
  }
  `,
    {
      variables: {
        query: `data/Slug/iv eq '${params.slug}'`,
      },
    }
  );

  if (data.queryPostsContents.length === 0) {
    return {
      notFound: true,
    };
  }
  const post = data.queryPostsContents[0].flatData;

  const mdxSource = await serialize(post.content);

  return {
    props: {
      post: post,
      source: mdxSource,
    },
  };
}
```

This returns data for the blog post associated with a particular slug or returns a 404 error if no such post exists.

At this point, you may receive an error because when you use `getStaticPaths` with dynamic paths in Next.js, you are required to also export a `getStaticPaths` function that tells Next.js [what pages to generate at build time](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation).

Export a `getStaticPaths` function in `squidex-blog/pages/index.js`:

```js
export async function getStaticPaths() {
  const data = await fetchAPI(`
  {
    queryPostsContents {
      id,
      flatData {
        slug
      }
    }
  }
  `);

  return {
    paths: data.queryPostsContents.map((post) => {
      return {
        params: {
          slug: post.flatData.slug,
        },
      };
    }),
    fallback: false,
  };
}
```

The Next.js development server only runs `getStaticProps` to fetch the page data when the page is initally loaded. So, to preview the changes made to `getStaticProps`, you will refresh the page. When you do so, the data will be made available but since it has not been rendered, nothing will be visible. You will now proceed to render the blog post on this page.

Modify the `BlogPost` function in `[slug].js`:

```jsx
export default function BlogPost({ post, source }) {
  return (
    <div>
      <h1 className="blog-post-title">{post.title}</h1>
      <MDXRemote {...source} />
    </div>
  );
}
```

Once you save the file, the title of the blog post and the body of the post will be rendered on the page. If you did not refresh the page after adding `getStaticProps`, you may see an error because the page data has not been loaded. Refresh the page to fetch data from your Squidex application.

The finished `[slug].js` file will look like this:

```jsx
import { serialize } from "next-mdx-remote/serialize";
import { MDXRemote } from "next-mdx-remote";
import fetchAPI from "../lib/squidex";

export default function BlogPost({ post, source }) {
  return (
    <div>
      <h1 className="blog-post-title">{post.title}</h1>
      <MDXRemote {...source} />
    </div>
  );
}

export async function getStaticProps({ params }) {
  const data = await fetchAPI(
    `
    query QueryPosts($query: String!) {
      queryPostsContents(filter: $query) {
        id
        flatData {
          title
          content
        }
      }
    }
    `,
    {
      variables: {
        query: `data/Slug/iv eq '${params.slug}'`,
      },
    }
  );

  if (data.queryPostsContents.length === 0) {
    return {
      notFound: true,
    };
  }
  const post = data.queryPostsContents[0].flatData;

  const mdxSource = await serialize(post.content);

  return {
    props: {
      post: post,
      source: mdxSource,
    },
  };
}

export async function getStaticPaths() {
  const data = await fetchAPI(`
    {
      queryPostsContents {
        id,
        flatData {
          slug
        }
      }
    }
    `);

  return {
    paths: data.queryPostsContents.map((post) => {
      return {
        params: {
          slug: post.flatData.slug,
        },
      };
    }),
    fallback: false,
  };
}
```

You have now built a fully functioning blog in Next.js that sources data from Squidex.

## Conclusion

Now that you have built your blog with Next.js and Squidex, you may want to know how to deploy your blog to a live site. Check out this page on [Deploying a Next.js site](https://nextjs.org/docs/deployment). You may also want to explore the [Next.js Head component](https://nextjs.org/docs/api-reference/next/head) to see how to adding meta tags to your pages to allow search engines better understand your content.
