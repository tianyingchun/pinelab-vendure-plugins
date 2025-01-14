# Vendure Google Asset Storage plugin

![Vendure version](https://img.shields.io/npm/dependency-version/vendure-plugin-google-storage-assets/dev/@vendure/core)

### [Official documentation here](https://pinelab-plugins.com/plugin/vendure-plugin-google-storage-assets)

Plugin for storing Vendure assets on Google Cloud Storage

## Getting started

1. `yarn add vendure-plugin-google-storage-assets`
1. Create a bucket which is publicly available in Google Cloud.
1. Add to your `sendcloud.dev-config.ts`

```ts
import { GoogleStoragePlugin, GoogleStorageStrategy } from 'vendure-plugin-google-storage-assets'

plugins: [
  AssetServerPlugin.init({
    storageStrategyFactory: () => new GoogleStorageStrategy({
      bucketname: 'your-bucket-name',
      /**
       * Use to pre-generate thumbnail sized images.
       * Thumbnails are available on product.featured_asset.thumbnail via GraphQL
       */
      thumbnails: {
        width: 500,
        height: 500,
      },
      /**
       * You can set this to 'false' to make the Vendure admin ui also consume images directly
       * from the Google Cloud Storage CDN,
       * instead of via the Vendure asset server
       */
      useAssetServerForAdminUi: false
    }),
    route: 'assets',
    assetUploadDir: '/tmp/vendure/assets',
    port: 3001,
  }),
  GoogleStoragePlugin,
```

### Local development

For local development, use `gcloud auth application-default login` to authorize for your Gcloud project.  
Internally this plugin uses `new Storage();` to instantiate the Storage client, which uses ENV variables to
authenticate:

```
// By default, the client will authenticate using the service account file
// specified by the GOOGLE_APPLICATION_CREDENTIALS environment variable and use
// the project specified by the GCLOUD_PROJECT environment variable. See
// https://cloud.google.com/docs/authentication/production#providing_credentials_to_your_application
```

https://cloud.google.com/compute/docs/tutorials/nodejs-guide
