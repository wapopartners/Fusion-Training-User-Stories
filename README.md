# The Washington Post's Arc Outbound Feeds skeleton

This is a fusion themes based repository and is intended to be used as the starting point for enabling _out of the box and custom Arc Outboundfeeds_.

## Setup

Pre-requisites:

- node / npm installed (node version > 10).
- github personal access token with `read:packages` access and sso enabled for WPMedia
- docker

1. Clone this repo:

```
git clone git@github.com:wapopartners/outboundfeeds-skeleton.git
```

2. Create a `.npmrc` in the projects root directory with your github access token. This file is in the .gitignore file and should never be checked into github.

```
@wpmedia:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken={ YOUR AUTH TOKEN HERE }
```

3. Install the packages

```
npm install
```

4. Create .env

   Copy env.example to .env and edit the file to replace the placeholders with your correct values.

   - CONTENT_BASE - Set your org in CONTENT_BASE
   - ARC_ACCESS_TOKEN - your readonly developer token. [ALC](https://redirector.arcpublishing.com/alc/arc-products/developer/user-documentation/accessing-the-arc-api/?product=)
   - resizerKey - your orgs resizerKey. If you don’t have it, please contact your Technical Delivery Manager (TDM)

   The .env file is in .gitignore and should never be checked into github.

Run Fusion locally see [here](https://redirector.arcpublishing.com/alc/arc-products/pagebuilder/fusion/documentation/recipes/running-fusion-locally.md) for more details:

```
npx fusion start
```

Once fusion has finished starting you should be able to to get to the pagebuilder editor [pages](http://localhost/pagebuilder/pages) and [templates](http://localhost/pagebuilder/templates)

Run tests with:

```
npm test
```

Run the linter with:

```
npm run lint
```

For more information on developing outbound feeds:

- [documentation](documentation/README.md)
