# WordPress Heroku

This project is a template for installing and running [WordPress](http://wordpress.org/) on [Heroku](http://www.heroku.com/). The repository comes bundled with:
* [Amazon S3 and Cloudfront](https://wordpress.org/plugins/amazon-s3-and-cloudfront/)
* [WP Sendgrid](https://wordpress.org/plugins/wp-sendgrid/)
* [Wordpress HTTPS](https://wordpress.org/plugins/wordpress-https/)

## Installation

1. Clone the app (i.e. this repo).

  ```bash
  git clone https://github.com/18F/cf-ex-wordpress.git cf-ex-wordpress
  cd cf-ex-wordpress
  ```

1. Create a service instance of a MySQL Database
View Services  
`cf marketplace`  
View Specific Service Plans  
Template: `cf marketplace -s SERVICE`  
Example: `cf marketplace -s mysql56`  
Create Service Instance  
Template: `cf create-service SERVICE PLAN SERVICE_INSTANCE`  
Example: `cf create-service mysql56 free mysql-service`  

2. Create a service instance of S3
Template: `cf create-service SERVICE PLAN SERVICE_INSTANCE`  
Example: `cf create-service s3service free SERVICE_INSTANCE`  

3. Edit the manifest.yml file.  Change the 'host' attribute to something unique.  Then under "services:" change "mysql-service" to the name of your MySQL service.  This is the name of the service that will be bound to your application and thus used by Wordpress.

4. Deploy the app with a no start command
`cf push --no-start`

5. Set environment variables for secret keys using [Wordpress Secret Key Generator](https://api.wordpress.org/secret-key/1.1/salt/)
```bash
cf set-env mywordpress-new AUTH_KEY YOUR_KEY
cf set-env mywordpress-new SECURE_AUTH_KEY YOUR_KEY
cf set-env mywordpress-new LOGGED_IN_KEY YOUR_KEY
cf set-env mywordpress-new NONCE_KEY YOUR_KEY
cf set-env mywordpress-new AUTH_SALT YOUR_KEY
cf set-env mywordpress-new SECURE_AUTH_SALT YOUR_KEY
cf set-env mywordpress-new LOGGED_IN_SALT YOUR_KEY
cf set-env mywordpress-new NONCE_SALT YOUR_KEY
```

6. Push it to CloudFoundry.

```bash
cf push
```

## Usage

Because a file cannot be written to Cloud Foundry's file system, updating and installing plugins or themes should be done locally and then pushed to Cloud Foundry.

## Updating

Updating your WordPress version is just a matter of merging the updates into
the branch created from the installation.

    $ git pull # Get the latest

Using the same branch name from our installation:

    $ git checkout production
    $ git merge master # Merge latest
    $ git push heroku production:master

WordPress needs to update the database. After push, navigate to:

    http://your-app-url.herokuapp.com/wp-admin

WordPress will prompt for updating the database. After that you'll be good
to go.

## Deployment optimisation

If you have files that you want tracked in your repo, but do not need deploying (for example, *.md, *.pdf, *.zip). Then add path or linux file match to the `.slugignore` file & these will not be deployed.

Examples:
```
path/to/ignore/
bin/
*.md
*.pdf
*.zip
```

## Wiki

* [Custom Domains](https://github.com/mhoofman/wordpress-heroku/wiki/Custom-Domains)
* [Media Uploads](https://github.com/mhoofman/wordpress-heroku/wiki/Media-Uploads)
* [Postgres Database Syncing](https://github.com/mhoofman/wordpress-heroku/wiki/Postgres-Database-Syncing)
* [Setting Up a Local Environment on Linux (Apache)](https://github.com/mhoofman/wordpress-heroku/wiki/Setting-Up-a-Local-Environment-on-Linux-\(Apache\))
* [Setting Up a Local Environment on Mac OS X](https://github.com/mhoofman/wordpress-heroku/wiki/Setting-Up-a-Local-Environment-on-Mac-OS-X)
* [More...](https://github.com/mhoofman/wordpress-heroku/wiki)
