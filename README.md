# Lab 07
This lab focuses on fetching content from an API and dynamically piping content to a PageBuilder user. For more information on fetching content, [click here](https://redirector.arcpublishing.com/alc/arc-products/pagebuilder/fusion/documentation/recipes/fetching-content.md?version=2.7).

In order to fetch content from an API, we need to define what API we'll be fetching from and provide credentials. We will use your organizations Arc Content API to fetch data. This why we defined our CONTENT_BASE and ARC_ACCESS_TOKEN in `.env` during setup. 

1. With the `.env` we automatically have access to our orgs API, but now we need to create a content source to retrieve the API data. All content sources goes into the `content/sources` folder

2. Open the `/content/sources` folder and create a new file called `content-api.js`

3. Give your content source a schema name. This will be used later to define and connect a schema. For example:
```
const schemaName = 'article';
```

4. Are there params that need to be provided to use this content source? Then add them in:
```
const params = {
  website_url : 'text',
  published: 'text',
};
```

5. We need to define the URI which the content source will call. Create a fusion `resolve` function which takes in a `key` holding the params defined above and returns the URI. For more details on content source, [click here](https://redirector.arcpublishing.com/alc/arc-products/pagebuilder/fusion/documentation/recipes/defining-content-source.md?version=2.7)
```
const resolve = (key) => {
  const requestUri = `/content/v4/stories/?website_url=${ key.website_url || key }&website=${org}`;

  return (key.hasOwnProperty('published')) ? `${requestUri}&published=${key.published}` : requestUri;
}
```

6. *(Do this if you do not have access to your Arc organization)* As a work-around, download [this](https://github.com/wapopartners/Fusion-Training-User-Stories/blob/lab-08/helpers/sample-content.js) content from the next lab and store it in a new directory at the root of your porject called `/helpers`. Now, we can import this sample data into our content source and return it when this content source is called like below. NOTE: Here we use a `fetch` function instead of `resolve` as we are returning the final JSON instead of a URI.

```
import { CONTENT } from '../../helpers/sample-content';
const fetch = (query = {}) => {
  return CONTENT;
};
```

7. Finally, export your content source using either `resolve` or `fetch`:
```
export default {
  fetch,  // you only need this if using the sample data mentioned on step #6
  params,
  schemaName,
}
```
OR
```
export default {
  resolve,
  params,
  schemaName,
}
```

8. Since content sources usually return a lot of data, we need to define what fields we want in order to reduce the size and improve performance. This is done by creating a `schema` for our content source.

9. From root, open the `/content/schemas` folder - this is where all schemas reside. View the [schema documentation](https://redirector.arcpublishing.com/alc/arc-products/pagebuilder/fusion/documentation/recipes/using-graphql-schema.md) to learn more

10. Create a new JS file called `article.js` (the schemaName we defined in step 3). 

11. Here, we are going to define the fields we want from the content source. Fields important to us are:
```
export default `
  type Query {
    type: String!
    version: String!
    description: Description
    headlines: Headlines
    promo_items: PromoItems
    subheadlines: Subheadlines
  }
  ...
`
```

12. Notice how `description, headlines, promo_items and subheadlines` are referencing other types - this is because they hold objects. So we need to define those as well as part of the default export:
```
type BasicPromoItems {
  type: String
  url: String
}
type Description {
  basic: String
}
type Headlines {
  basic: String
}
type PromoItems {
  basic: BasicPromoItems
}
type Subheadlines {
  basic: String
}
```

13. Now that we have our content source and schema defined, we can test it out! Rebuild fusion and open your test page from previous tasks and scroll to the "Page Content" section

14. Here, you will see a drop down for "Global Content Service" - select your `content-api` and enter values for the `website_url` and `published` fields (a `website_url` should come from a published story in your organizations `sandbox` Composer tool). Save and publish the page.

15. We're going to provide a tip - you can view Fusion page data with the following JavaScript. Create a new bookmark on your browser called `View Fusion Data`, and paste the JavaScript in `/helpers/view-fusion-data.js` from this branch into the URL. More documentation on this [here.](https://redirector.arcpublishing.com/alc/arc-products/arcwide/user-docs/self-onboarding-bookmarklet/).

16. Open your test page and click on your new bookmark - you should see a panel on the left-hand side of your screen appear.

17. Click on "Global Content" and you'll see the content returned from your content source! You'll also be able to view the data that you originally set as your content source parameters - `website_url` and `published` fields.
<img width="500" alt="Screen Shot 2021-04-05 at 8 50 06 AM" src="https://user-images.githubusercontent.com/39777478/113575704-48bb7f80-95ec-11eb-814e-1e9de5b54d3f.png">

If you would like to test your content source independently of the page, you can navigate to PageBuilder > Developer Tools > Debugger. From here, select the site and Content Debugger. You will see a list of content sources you can click on and test.


## [Next up: Lab 08](https://github.com/wapopartners/Fusion-Training-User-Stories/tree/lab-08)
