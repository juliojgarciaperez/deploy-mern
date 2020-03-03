# deploy-mern

Let's Deploy our MERN (mongo, express, reaact, node) application using the following infrastructure stack:

* Heroku for node express API
* Heroku Mongolab add-on for MongoDB
* github pages for react web app
* OPTIONAL - Heroku for react web app

## 1. Heroku

Log in to heroku: https://id.heroku.com/login

Create a new app:

![Alt text](images/heroku_app.png?raw=true)

Install the Heroku CLI: 

https://devcenter.heroku.com/articles/heroku-cli

If you haven't already, log in to your Heroku account and follow the prompts to create a new SSH public key.

```
$ heroku login
```

go to your API repository and run the following (change super-trello-api with your heroku app name):

```
$ heroku git:remote -a super-trello-api
$ git push heroku master
```

Now your app is being deployed!

Now we'd need to add our environment variables.

Go to Settings

Click on Reveal Config Vars

Add your environment variables. You'll need to add the MONGODB_URI with the value of your database url (see next step), CORS, etc. _CHECK ALL YOUR ENVIRONMENT VARIABLES!!_.

From now, you can deploy your new commits to heroku by running:

```
$ git push heroku master
```

The thing is, if you access your server with `heroku open`, you'll get an application error. We can get more info by checking our heroku logs with `heroku logs`:

```
MongoError: failed to connect to server [localhost:27017] on first connect
```

Duh, we don't have a database yet!

## 2. Mongolab add-on

We can use the mongolab addon in heroku to deploy our database. We would do this by running the following command:

```
$ heroku addons:create mongolab:sandbox
```

This will fail if you haven't added a credit card to your heroku account. If you didn't add it for your second module project, go ahead and do that first! If you follow these steps, you won't get charged, as the Mongolab add-on is free. But be careful with other add-ons.

Now we just need to connect our backend with the database. For that, we need the connection string for the Mongolab database.
The Mongolab add-on adds an environment variable, `MONGODB_URI`. You can see it in the config section of your application in heroku, or by running the following command:

```
heroku config:get MONGODB_URI
```

This should output the full connection string. If you have already used `process.env.MONGODB_URI` in your API, you don't need to do anything else, it should be up and running! If you haven't, go ahead and add it in the `config/db.config.js` file of your API repository. Once you've done that, just commit and push to heroku, and the next time you access your API, it should be working!

## Github pages

Note: you'll need to use dynamic urls to connect to your heroku API instead of localhost. Be sure you've used react environment variables to store them. You can create both .env.local and .env.production files with different values (local points to localhost and production to heroku). React will use the production variables for builds.

Go to your web directory

Install gh-pages package

```
$ npm install --save-dev gh-pages
```

You'll need some changes in your react app to make it work with gh-pages.

If you are using react-router-dom, add the following to the <Router> or <BrowserRouter> component (at index.js):

```
<Router basename={process.env.PUBLIC_URL}>
```

First, add the following to package.json:

your github url will be:

https://<username>.github.io/<repository_name>

```
...
"homepage": "YOUR_GITHUB_PAGES_URL_HERE!",
...
```

Also, add this to the "scripts" section in package.json:

```
{
   ...
   "predeploy": "npm run build",
   "deploy": "gh-pages -d build",
   ...
}
```

Go to your repository settings

![Alt text](images/github_settings.png?raw=true)

At Github Pages section, select gh-pages branch as Source

Done!, check out your Github pages url

![Alt text](images/github_url.png?raw=true)

Done!, now just run `npm run deploy` to deploy your app yo github pages.

## 4. OPTIONAL - React App in Heroku

Note: you'll need to use dynamic urls to connect to your heroku API instead of localhost. Be sure you've used react environment variables to store them. You can create both .env.local and .env.production files with different values (local points to localhost and production to heroku). React will use the production variables for builds.

We are going to use an Heroku buildpack for create-react-app that will handles all the build and deploy processes.

You can follow this [tutorial](https://elements.heroku.com/buildpacks/mars/create-react-app-buildpack) at Heroku or here are the important steps:

Note: if you have added the gh-pages config (basename, hash-router, homepage at package-json...), just remove it.

Go to your web directory

Be sure you are logged to Heroku CLI or run: 
```
$ heroku login
```

Run the command:

```
$ heroku create $APP_NAME --buildpack mars/create-react-app
```

Where `$APP_NAME` is the name you app will have at heroku and at the deployed URL.

Now, be sure you have added and commited last changes.

Finally, push your repo to heroku:

```
$ git push heroku master
```

You will be seeing your terminal working right now. When it finishes, run:

```
$ heroku open
```

