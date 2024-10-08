# Project Overview

This project involves converting a static HTML website into a CMS-ready Next.js site using mocked API data. You will be working with a folder called `theme` that contains the HTML structure and using mock data from `pages/api/mock-api.json`. The goal is to dynamically generate pages using this CMS mock data, ensuring the site is ready for integration with a real CMS.

## Key Directories

- **theme/**: Contains the static HTML files you will convert to reusable Next.js components.
- **public/**: Holds all static assets (images, icons, etc.) to be referenced within the app.
- **pages/api/mock-api.json**: Contains mock CMS data, which will simulate the CMS behavior during development.
- **pages/[...permalink].js**: This file dynamically generates pages based on the `permalink` field in the mock API.

## Setup Instructions

1. **Install dependencies**:
   Run the following command to install necessary dependencies:
   ```bash
   npm install
   ```

2. **Folder Structure**:
   - `theme/`: Use the HTML files from the `theme` folder as a reference and convert them into Next.js components.
   - `pages/api/mock-api.json`: This file contains mock data resembling a real CMS. You will use this data to dynamically generate pages and render components.

3. **Rendering Pages**:
   - **[...permalink].js** is responsible for dynamically generating pages based on the `permalink` field from the CMS mock data.
   - Each page consists of multiple blocks, and these blocks are mapped to specific components based on the `name` field.

4. **Add New Components**:
   - Create new components for each section of the HTML structure in `theme/`.
   - Import the new components in `Render.js` and map the block `name` from the mock API to the component.
   - Example of adding a new component:
     ```js
     import NewComponent from './components/NewComponent';

     case 'NewComponent':
       return <NewComponent data={data} />;
     ```

5. **getStaticPaths**:
   Generates static pages during build time based on the `permalink` field from the mock API.

6. **getStaticProps**:
   Fetches the appropriate page data for each `permalink` and passes it as props to render the dynamic page content.

## Working with Head Component

- **Migrating Head Elements**:
   - Move all `<title>`, `<meta>`, and other head elements from the HTML into the `<Head>` component.
   - For example, if the theme has:
     ```html
     <head>
       <title>My Page</title>
       <meta name="description" content="Sample description">
       <link rel="icon" href="/favicon.ico">
     </head>
     ```
     Convert it to:
     ```jsx
     <Head>
       <title>{title}</title> {/* Use dynamic title from mock API */}
       <meta name="description" content="Sample description" />
       <link rel="icon" href="/favicon.ico" />
     </Head>
     ```

- **Dynamic Meta Tags**:
   - If the CMS mock data contains dynamic meta tags (like `global_tags`), render them inside the `<Head>` component.
   ```jsx
   <Head>
     <title>{title}</title>
     {global_tags.map(tag => (
       <meta key={tag.attributes.key} name={tag.attributes.key} content={tag.attributes.content} />
     ))}
   </Head>
   ```

## Public Directory for Static Assets

- **Moving Static Files**:
   - All static assets like images, fonts, and icons should be placed in the `public/` directory.
   - Update image references in your components to point to the public directory. For example:
     ```html
     <img src="/assets/logo.png" alt="Logo">
     ```

- **Favicon**:
   - Move favicon files into `public/` and update the `<Head>` component to use the correct path:
     ```jsx
     <Head>
       <link rel="icon" href="/favicon.ico" />
     </Head>
     ```

## Handling Images with Defaults

When rendering images from the mock API, always provide a fallback image if the image URL is not available. You can store a default image in the `public/` folder and reference it whenever necessary.

### Example of Image Fallback

```jsx
import React from 'react';

function ExampleComponent({ data }) {
  const defaultImage = '/assets/default-image.png'; // Path to the default image in public/

  return (
    <div>
      <img src={data.image?.value || defaultImage} alt={data.image?.alt || 'Default Image'} />
    </div>
  );
}

export default ExampleComponent;
```

In this example, if the `data.image.value` is null or undefined, it falls back to `defaultImage`, ensuring that the site always displays a valid image.

## Notes for Development

- **API Data Comments**: Whenever you encounter something that might be off in the API data or a component, please add a comment starting with `note:`. This will help identify potential issues or inconsistencies.
- **Handling Visuals**: Try to keep consistency with the photos provided in each file. Tailwind is available for minor modifications to the styling.

## Checklist for Migration

- **Head Elements**: Migrate titles, meta tags, and favicons into the `<Head>` component.
- **Static Assets**: Move all images, fonts, and other static assets into the `public/` directory.
- **Image Fallbacks**: Ensure all images use a default if the CMS data doesn't provide an image.
- **Components**: Create reusable components based on the HTML structure in the `theme` folder and map them to blocks in the mock API.
- **Dynamic Pages**: Ensure pages are dynamically generated based on the mock CMS data using `getStaticPaths` and `getStaticProps`.

## Conclusion

By following this guide, you’ll convert the static HTML theme into a fully dynamic Next.js website ready to be connected to a CMS. Make sure to use the mock data effectively and add fallback mechanisms (like default images) to handle missing data gracefully.

Feel free to reach out if anything is unclear or if you need help with any part of the migration!