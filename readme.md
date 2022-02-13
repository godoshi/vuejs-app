# Requirements

node.js 8.16.0
npm 8.3.1

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

## Deployment
Deployed using AWS S3 Static hosting, for initial setup follow this guide: https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html

1. Update `.env` file variable `VUE_APP_API_URL` to the api endpoint based on the environment you're deploying to
2. Build the app
```
npm run build
```
3. Upload the contents of the `dist/` folder to the S3 bucket


## TODO
- contanierize local dev environment
- break up `App.vue` into reusable components
- authentication
- automate deployments, CI/CD
- unit testing with jest
- be on the lookout to upgrade vue.js & vuetify
- add vue router and vuex if complexity grows


### Helpful Links
- [nvm](https://github.com/nvm-sh/nvm)
- [vue.js](https://vuejs.org/)
- [vue-cli](https://cli.vuejs.org/)
- [vuetify](https://vuetifyjs.com/en/)
- [axios](https://axios-http.com/)
