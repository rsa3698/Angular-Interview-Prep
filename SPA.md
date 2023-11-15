1. Angular Single Page Applications (SPA): What are the Benefits?
   

Section Summary – Advantages of SPAs and the Key Concept About How They Work
Building our application as a SPA will give us a significant number of benefits:

We will be able to bring a much-improved experience to the user

The application will feel faster because less bandwidth is being used, and no full page refreshes are occurring as the user navigates through the application

The application will be much easier to deploy in production, at least certainly the client part: all we need is a static server to serve a minimum of 3 files: our single page index.html, a CSS bundle, and a Javascript bundle.

We can also split the bundles into multiple parts if needed using code splitting.

The frontend part of the application is very simple to version in production, allowing for simplified deployment and rollbacks to previous version of the frontend if needed

2. SPA and SEO? 
One of the drawbacks of Single Page Applications (SPAs) in the context of SEO (Search Engine Optimization) is the initial challenge they pose to search engine crawlers. Here are the key drawbacks:

Limited Initial Crawling:

Search engine crawlers, like those used by Google, initially struggle with indexing SPAs because they traditionally rely on parsing static HTML content. SPAs often load content dynamically using JavaScript, making it harder for crawlers to discover and index all the relevant content during the initial crawl.
Potential Delay in Indexing:

Since SPAs load content asynchronously, there might be a delay in search engines indexing new content. Search engines may not immediately recognize updates made via JavaScript, affecting the speed at which changes in your application are reflected in search results.
SEO Metadata Challenges:

Traditional SEO techniques heavily rely on HTML metadata (e.g., title tags, meta descriptions). SPAs may not have all the metadata readily available in the initial page load, as it could be dynamically generated after additional data is fetched. This can impact how search engines interpret and rank your content.

3. How SPA works?

The Way SPAs Work
The way that single page applications bring these benefits is linked to the way that they work internally:

After the startup, only data gets sent over the wire as a JSON payload or some other format. But no HTML or CSS gets sent anymore over the wire after the application is running.
The key point to understand how single page applications work is the following:

instead of converting data to HTML on the server and then send it over the wire, in a SPA we have now moved that conversion process from the server to the client.

The conversion happens last second on the client side, which allow us to give a much improved user experience to the user.