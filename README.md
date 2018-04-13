# Create a Staging Environment for your Heroku Apps

We will use Heroku Pipelines
I assume that you already have a rails app running on your local machine.

## 1. Create an App on heroku (if it is not done yet)

  If you are using Heroku CLI you can type the commands listed here. Of course every command can be done via your Heroku   Dashboard interface.
  
  To start lets create an Heroku App`$ heroku create MY_APP`
  
  * *note that you can set useful things like an App name or a region*
  
## 2. Create a Pipeline

  We create a pipeline specifying the App `$ heroku pipelines:create -a MY-APP` 
  
  * you will be prompt for settings :
  
   + a pipeline name (* *it can be your App name*)
   + the stage of your app between development, staging and production. (* *You should probably chose production if you started from your main App*)
    
## 3. Create your Staging App 

  `$ heroku create MY-APP-STAGING --remote staging` 
  
  * *note that you can also add some options same as Step 1*
  
  We are setting another remote so as to be able to deploy from CLI. 
  You can also set an automatic deployment later on.
  
## 4. Add the staging App to your pipeline

  `$ heroku pipelines:add -a MY-APP-STAGING PIPELINE_NAME` 
  
  
  **note that `PIPELINE_NAME` is the one set on Step 2 if you didn't chose it is probably the same name as you production App*
  
  You will be prompt for your App Stage, this one should be `staging`.
  
## Done ! 

You should now go to your Staging App config Vars and set a new one `KEY: RAILS_ENV, VALUE: staging`
this step is not mandatory and this will probably generate a Warning from Heroku when you deploy this App, but it will 
let you use checks like `Rails.env.staging?` in your code if you want different action based on environment.

## How to use it when done ?

Now to deploy you have to push on the staging remote `$ git push staging master`

Once you're happy with your App on staging it's time to promote it `$ heroku pipelines:promote -r staging`

Here you are ! 
  
I hope this could help some of you testing your Apps in an actual production-like environment before releasing.
Of course do not hesitate to ping me if something is not clear enough and feel free to contribute to this gist.
  
Useful Links : 
  + Create Apps from Heroku CLI : https://devcenter.heroku.com/articles/creating-apps
  + Manage Heroku Pipeline : https://devcenter.heroku.com/articles/pipelines

