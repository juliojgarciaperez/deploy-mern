# deploy-mern

Let's Deploy our MERN (mongo, express, reaact, node) application using the following infrastructure stack:

* Heroku for node express API
* Mlab for MongoDB
* github pages for react web app

## 1. Mlab

Log in to mlab: https://mlab.com/login/

Create a new database: https://mlab.com/create/wizard

  - Select Amazon as Cloud Provider and SANDBOX (free) at Europe (Ireland) for the plan type
  - Select a database name (I'd use your app name)

Look at this order confirmation example:

![Alt text](images/mlab_order_confirmation.png?raw=true)

Submit your order!

Wait a few seconds for the database to be created. Then click on the database name or go to the url https://mlab.com/databases/<database_name>

Now we'll need to create an user for our database.

Click on "Users"
Click on "Add database user"
Fill the form (please remember the password!!!)

Nice! We'd need the url to connect to our database, you can copy it from the Mlab website:

![Alt text](images/mlab_url.png?raw=true)

## 2. Heroku

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

Done! your app is being deployed!

Now we'd need to add our environment variables.

Go to Settings

Click on Reveal Confir Vars

Add your environment variables. You'll need to add the MONGODB_URI with the value of your Mlab url, CORS, etc. _CHECK ALL YOUR ENVIRONMENT VARIABLES!!_.

From now, you can deploy your new commits to heroku by running:

```
$ git push heroku master
```

## Github pages

Note: you'll need to use dynamic urls to connect to your heroku API instead of localhost. Be sure you've used react environmnet variables to store them. You can create both .env.local and .env.production files with different values (local points to localhost and production to heroku). React will use the production variables for builds.

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
