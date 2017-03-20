<div align="center">
<h1>Capstone Demo App<em> Module 1</em></h1> 
<h2>Software Development Documentation</h2>
  <h3>
    <a href="#">
      Module 1
    </a>
    <span> | </span>
       <a href="#">
      Module 2
    </a>
    <span> | </span>
       <a href="#">
      Module 3
    </a>
    <span> | </span>
       <a href="#">
      Module 4
    </a>
    <span> | </span>
       <a href="#">
      Module 5
    </a>
    <span> | </span>
           <a href="#">
      Module 6
    </a>
    <span> | </span>
      <a href="#">
        Module 7
      </a>
    <span> | </span>
      <a href="#">
        Module 8
      </a>
  </h3>
</div>

<div align="center">
  <sub>
  <a href="https://github.com/appwebtech">Joseph M Mwania</a> Ruby on Rails Specialisation Capstone
  </a>
</div>


<h2>Table of Contents</h2>
- [Introduction](#introduction)
  - [Server Side](#server-side)
  - [Client Side](#client-side)
- [Technical Architecture Diagram](#technical-architecture-diagram)
- [DB and File Storage](#ruby-on-rails-web-services-and-integration-with-mongodb)
- [Application Frameworks and Languages](#application-frameworks-and-languages)
- [Coding Environment](#coding-environment)
- [App Setup](#app-setup)
- [DB yml environment set-up](#db-yml-environment-set-up)
- [What the App should do](#what-the-app-should-do)
- [RDBMS Backed Resource](#rdbms-backed-resource)
- [MongoDB Backed Resource](#mongodb-backed-resource)
- [Regression Testing](#regression-testing)
- [Web Service Finishing Touches](#web-service-finishing-touches)
    - [RDBMS side](#rdbms-side)
    - [Mongo side](#mongo-side)
- [CORS](#cors)
- [API Deployment](#api-deployment)

## Introduction 
In this module, I'll code and build some stuff along as the instructor, [Eng. Jim Stafford](https://www.linkedin.com/in/jim-stafford-6974aa8) delivers the lectures. Due a number of technologies and their capabilities I intend to put into practice during the entire capstone, I'll try my best to be brief and at the same time clear without making stuff look like an alphabet soup. 

I'll be going through the **Technical** and **Deployment Architectures**. The former encompasses *Layers* and *Capabilities and Technologies* whilst the latter is more on *API and Database* and *Web Client*.

I'll be developing from bottom to top as per the materials provided in the lectures, covering seven areas within the applications I'll develop from the first module up to the eighth. I'll then follow two areas of development and testing and wrap up with deployment.

On the RDBMS, I'll use SQLite for development and PostgreSQL (PG) for deployment. SQLite is ideal for applications with a low dependency on SQL constructs while PG is a fully supported production DB. I'll use PG for both development and deployment in order to avoid fixing stuff later upon deploying. 

The interface to the DB is **ActiveRecord** which puts a nice *Object/Relational Mapper* as I input objects and methods whilst coding. Sometimes I'll drop complex queries in SQL or even build snippets of WHERE clauses and SELECT clauses or even do full-scale raw SQL commands which are more efficient. Where I deem fit I'll make snapshots of Bash available but only in rare instances as this documentation may become too long. 

I'll use the MongoDB (No SQL) which uses Mongoid as the object document mapper. It provides a nice ActiveRecord like interface to the back-end document store. I won't use GridFS but BSON::Binary type which I'll use to store in documents. 

Those documents will be related to the RDBMS which has the business object that has data, eg photo on a file. In Mongo, I'll store the actual content i.e photo as in [Course 3](https://github.com/appwebtech/Ruby_on_Rails_Certification/tree/master/3_Ruby_on_Rails_Web_Services_and_Integration_with_MongoDB/MODULE%2002).
Later on, I'll use Google geo code API in order to find positioning. 

On the Browser side, I won't use file storage, but on the contrary, a module; the ng-token-auth module which will store current login info into the Browser Cookie store to avoid successive re-authentication. 

As I progress within the capstone, I'll try my best to document my experiences with both the materials (gems, applications, etc) suggested by the instructors and the additional ones I may personally include. 

The application frameworks and languages used are as follows; 

### Server Side
1. Rails API gem 
2. Rails 
3. WEBrick / Puma / ActiveModel 
4. Ruby 

### Client Side 
1. AngularJS 
2. Javascript 
3. HTML 
4. CSS 

## Technical Architecture Diagram
![screen shot 2017-02-18 at 14 37 10](https://cloud.githubusercontent.com/assets/20464709/23093628/ebdf656c-f5e8-11e6-838d-fa23fd6411dc.png)
<hr>

## DB and File Storage
![screen shot 2017-02-18 at 14 57 04](https://cloud.githubusercontent.com/assets/20464709/23093689/9eeca876-f5ea-11e6-9b7c-d46a1eb71f82.png)
<hr> 

## Application Frameworks and Languages 
![screen shot 2017-02-18 at 15 43 28](https://cloud.githubusercontent.com/assets/20464709/23094000/4a5bb9bc-f5f1-11e6-844c-82b7ed9355a4.png) 
<hr>

## Coding Environment

```
- macOS Sierra Version 10.12.2(16C67) (Running on an iMac)

- OS X El Capitan Version 10.11.6 (Running on a Macbook Pro)
  (Using different specs eg Rails 5, Puma, etc for comparison purporses)

- Firefox (v45.7.0 ESR) ~ Extended Support Release

- Ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin16]

- Rails 5.0.2 

- Rails 4.2.6  (Will scaled back from rails 5.0.2 to curb esoteric issues)

- Rails-api '~>0.4', '>=0.4.0'

- Homebrew 1.1.9

- rbenv 1.1.0 

- Git v2.11.1 

- PostgreSQL 9.6.1

- Mongo DB version v3.4.2

- NodeJS v6.6.0

- ImageMagick 7.0.4-7 Q16 x86_64 2017-02-04

- PhantomJS v2.1.1

- Chrome Driver for selenium 
  2.27.440174 (e97a722caafc2d3a8b807ee115bfb307f7d2cfd9)

- Editor: Sublime text 3 

```

## App Setup

My RDBMS choice is PG due to it's robustness and production capability (Requires DB server Installation). It's consistent with development and deployment, as opposed to SQLite (No DB Requirement) which is a functional lightweight administration. 

Using Full Rails server sucks due to it's "full kitchen sink" of JSON's, Views, Asset Pipeline, ERB, etc. What I've done is use an API with client-side web pages (JS, Angular etc); the server (Webrick) will provide API to client-side web pages. I'm versatile with it as I can turn on features that I want instead of overloading my workflow. 

## DB yml environment set-up

```ruby
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  username: <%= ENV['POSTGRES_USER'] %>
  password: <%= ENV['POSTGRES_PASSWORD'] %>
  host: <%= ENV['POSTGRES_HOST'] %>

development:
  <<: *default
  database: MODULE_01_development

test:
  <<: *default
  database: MODULE_01_test


production:
  adapter: postgresql
  encoding: unicode
  pool: 5
  url: <%= ENV['DATABASE_URL'] %>

```

 In a nutshell:
 - I chose RDBMS (PostgreSQL)
 - I chose Rails API as my server.
 - I Instantiated the App 
 - I readied the RDBMS schema 
 - I re-enabled the View 

## What the App should do

The App is RDBMS and MongoDB backed, and thus should create RDBMS & MongoDB backed models and expose RDBMS & MongoDB backed API resources. 

What I'll do is install rspec and change the **api_development_spec.rb** from it's default installations to accommodate our DB's. 

```ruby
# Do away with this 
RSpec.describe "ApiDevelopments", type: :request do
  describe "GET /api_developments" do
    it "works! (now write some real specs)" do
      get api_developments_path
      expect(response).to have_http_status(200)
    end
  end
end

# Add this 

RSpec.describe "ApiDevelopments", type: :request do
  describe "RDBMS-backed" do
    it "create RDBMS-backed model"
    it "expose RDBMS-backed API resource"  
  end

   describe "MongoDB-backed" do
    it "create MongoDB-backed model"
    it "expose MongoDB-backed API resource"  
  end
end

```

Haven't done much here. Just written some simple tests to see if RDBMS and NoSQL are configured and working well. 

## RDBMS Backed Resource

Created tests for the RDBMS-backed model and exposed to API resource, then ran tests with rspec. Corrected errors until tests went green.

```ruby
RSpec.describe "ApiDevelopments", type: :request do
    def parsed_body
        JSON.parse(response.body)
    end

  describe "RDBMS-backed" do
    before(:each) { Josembi.delete_all }
    after(:each) { Josembi.delete_all }

    it "create RDBMS-backed model" do 
        object=Josembi.create(:name=>"test")
        expect(Josembi.find(object.id).name).to eq("test")
    end

    it "expose RDBMS-backed API resource"  do
        object=Josembi.create(:name=>"test")
        expect(josembis_path).to eq("/api/josembis")
        get josembi_path(object.id)
        expect(response).to have_http_status(:ok)
        expect(parsed_body["name"]).to eq("test")
    end
  end

```


For a jump-start on this application, I generated a scaffold, with the model name of Josembi and a property of name. Due to RDBMS, the --orm points to active_record. This generated a model class and a schema migration, subsequent of which I populated the DB with a new table called Josembis that represent the model.

The Controller generated has the ability to CRUD RESTfull objects with the routing created. Then I did customizations as the /api/josembis URI wasn't working. Marvelously, HTTP methods of GETs, POSTs, PUT/PATCHes and DELETEs are working with a full control of which methods are in use and which ones are not. 

I then added some data to the record via the rails console, then called it to **View** on the browser. 
**VIEW** => Sends HTTP Request via */api/josembis* to Controller in my local server. 
Controller class **JosembisController** via it's actions *(:index, :show, create, :update)* sends request to **Model** (*class Josembi*), which will fetch data in the table I created in the rails console. 

I have an external software from google which builds json data for me. So in between the **URI/METHOD Routes** and **Controller**, an *index.json.jbuilder* & *show.json.jbuilder* was triggered to generate *_josembi.json.jbuilder*, which is what was outputted in **View**.

```ruby
class JosembisController < ApplicationController
  before_action :set_josembi, only: [:show, :update, :destroy]

  # GET /josembis
  # GET /josembis.json
  def index
    @josembis = Josembi.all

  #  render json: @josembis
  end

  def show
   # render json: @josembi
  end

  def create
    @josembi = Josembi.new(josembi_params)

    if @josembi.save
     # render json: @josembi, status: :created, location: @josembi
      render :show, status: :created, location: @josembi
    else
      render show: @josembi.errors, status: :unprocessable_entity
    end
  end

  def update
    @josembi = Josembi.find(params[:id])

    if @josembi.update(josembi_params)
      head :no_content
    else
      render json: @josembi.errors, status: :unprocessable_entity
    end
  end

  def destroy
    @josembi.destroy

    head :no_content
  end

  private
    def set_josembi
      @josembi = Josembi.find(params[:id])
    end
    def josembi_params
      params.require(:josembi).permit(:name)
    end
end
```

In a nutshell:
 - I build skeletal resource
 - I updated rails-api controller to delegate to the view
 - I updated the route to scope below /api 

## MongoDB Backed Resource

Created tests for MongoDB-backed model exposing them to the API resource. 

```ruby
describe "MongoDB-backed" do
    it "create MongoDB-backed model" do
      object=FactoryGirl.create(:kasyula, :name=>"test")
      expect(Bar.find(object.id).name).to eq("test")
    end
    
    it "expose MongoDB-backed API resource" do
      object=FactoryGirl.create(:kasyula, :name=>"test")
      expect(bars_path).to eq("/api/bars")
      get bar_path(object.id) 
      expect(response).to have_http_status(:ok)
      expect(parsed_body["name"]).to eq(object.name)
      expect(parsed_body).to include("created_at")
      expect(parsed_body).to include("id"=>object.id.to_s)
    end
  end
end
```

Failed test as in the case od RDBMS after running rspec. Generated a controller with --orm mongoid and got a partial parse. Mongoid specific prompts fail and other non Mongoid prompts parse. Rolled back the controller and installed **mongoid**, subsequent to which I generated a mongoid.yml file.

I hard coded *mongoid.yml* by supplying a mongo host environment variable so that I can access it locally from my server. For production, I set everything to a single variable, a URI and made an environment variable (MLAB_URI), a value that comes from MLAB pointing to the mongo instance. 


```erb 
development:
  clients:
    default:
      database: module01_development
      hosts:
        - <%= ENV['MONGO_HOST'] ||= "localhost:27017" %> 
      options:
  options:
test:
  clients:
    default:
      database: module01_test
      hosts:
        - <%= ENV['MONGO_HOST'] ||= "localhost:27017" %>
      options:
        read:
          mode: :primary
        max_pool_size: 1
production:
    clients:
        default:
            uri: <%= ENV['MLAB_URI'] %>
            options:
                conect_timeout: 15
```


I then loaded the *mongoid.yml* in the application file, making it the default ORM, which can be deactivated by the lines of code I added after the Mongoid one liner. 

code:  Mongoid.load!('./config/mongoid.yml')

Had issues running tests and excluded "Factory Girl rails". Rewrote mongo tests to look like below and parsed. 

```ruby
       describe "MongoDB-backed" do
    before(:each) { Kasyula.delete_all }
    before(:each) { Kasyula.delete_all }

    it "create MongoDB-backed model" do
      object=Kasyula.create(:name=>"test")
      expect(Kasyula.find(object.id).name).to eq("test")
    end
    
    it "expose MongoDB-backed API resource" do
      object=Kasyula.create(:name=>"test")
      expect(kasyulas_path).to eq("/api/kasyulas")
      get kasyula_path(object.id) 
      expect(response).to have_http_status(:ok)
      expect(parsed_body["name"]).to eq("test")
      expect(parsed_body).to include("created_at")
    end
  end
```


Later on I had to tweak kasyula controller to get mongoid well connected and running. Thats when my tests via rspec ran successfully and parsed. 

![screen shot 2017-02-12 at 10 15 40](https://cloud.githubusercontent.com/assets/20464709/22860942/7cdd8666-f10c-11e6-9f83-9516994ac6b1.png)
<hr>

And there we are with a false positive. 

In a nutshell:
 - I integrated MongoDB into application using Mongoid 
 - I generated the skeletal resource tailored towards Mongoid
 - I updated the marshaling behavior of JSON view

## Regression Testing

Previously, I made an end-to-end RDBMS and an end-to-end Mongo DB resource. Next what I will do is an overall application test just to be sure that one didn't break the other. I'll implement an automated test to verify this features then finish with automated regression test of the entire app. 

I've tested the whole app with rspec and everything DOES NOT look OK, so next, I'll need to investigate and search for solutions by accessing the  RDBMS and Mongo sides. 

## Web Service Finishing Touches 

I've added HTTParty in the development gems group, so it's time to party hard :-). Anyways, I've implemented a web service client to do various ad-hoc scriptings through the HTTP interface to add, remove and query resources. I've added and queried some data via IRB in bash (see an example below).

![screen shot 2017-02-13 at 21 28 16](https://cloud.githubusercontent.com/assets/20464709/22902621/f5e4e7fa-f235-11e6-86bd-bde8a067f9a7.png)
<hr>

Instead of doing SQL querying, I can still view the data via the browser in JSON. 

![screen shot 2017-02-13 at 21 50 10](https://cloud.githubusercontent.com/assets/20464709/22902764/72691828-f236-11e6-85e1-f2fb924fa31a.png)
<hr>

### RDBMS side
Used arguments to Query Foo, add some data check response. 

![screen shot 2017-02-26 at 11 04 06](https://cloud.githubusercontent.com/assets/20464709/23338753/96837a96-fc13-11e6-9a21-e45efdb0d7a3.png)
<hr>

![screen shot 2017-02-26 at 11 04 54](https://cloud.githubusercontent.com/assets/20464709/23338754/9967ab06-fc13-11e6-8bb4-184ca7038a17.png)
<hr>

## Mongo side
Check stored data, and use **HTTParty** to implement a web service client to do various ad-hoc scriptings through the HTTP interface to add, remove, and query resources.

![screen shot 2017-02-26 at 11 12 58](https://cloud.githubusercontent.com/assets/20464709/23338808/9c057e78-fc14-11e6-9158-1904ef969a8f.png)
<hr>

As above, I can listen to the server, POST request, DELETE request etc, as part of CRUD in development mode using rails c; it's very powerful, but a load of mumbo jumbo that I won't post here. However, I can show how to narrow down and use an ID of a parsed body and eventually delete it if need be. *See below for a small caption.*

![screen shot 2017-02-26 at 12 07 16](https://cloud.githubusercontent.com/assets/20464709/23339191/12a9d97c-fc1d-11e6-9a75-a38789bf8459.png)
<hr> 

## CORS 

I'll go deeper and implement the app to comply with the browser requirement of **same origin policy**; I'll also implement some **Cross-Origin Resource Sharing** with the server. This is vital if you want a dual deployment approach, where the server is deployed to Heroku and the UI is deployed to GitHub.

I've added the **rack-cors** gem to enable me look at some security issues presented to browsers where APIs and UI code are deployed independently. 

code: 'rack-cors', '~>0.4', '>=0.4.0', :require => 'rack/cors'

Before I get to Deployment, I have to FIX any code that is not parsing. By running rspec both globally and targeting the two databases in full documentation mode (-fd), I find some minor errors in the view which I fix and refactor my code. 

I've installed *pry shell* which is user-friendly compared to rails c which is lugubrious in my opinion; it's a question of personal opinions and tastes anyway.

Running rspec whilst targeting RDBMS in -fd mode. Found a link error which I fixed.
![pg access shell](https://cloud.githubusercontent.com/assets/20464709/23378642/19e7a954-fd34-11e6-81c3-27d4e90979b9.png)
<hr>

Running rspec in global scope. Fixed some esoteric errors on View, and fixed routes to render JSON which was been bypassed by html.erb. Rails 5.0.1 defaults to HTML within the view. We need BSON to transfer data within our Mongo.  

![screen shot 2017-02-26 at 10 31 12](https://cloud.githubusercontent.com/assets/20464709/23378727/763e5914-fd34-11e6-9885-783e08005e79.png)
<hr>

After the fixes, I re-ran rspec once again and code parsed both globally and within the RDBMS and Mongo targeted instances. Cool! Ready to deploy. 

![screen shot 2017-02-26 at 10 30 14](https://cloud.githubusercontent.com/assets/20464709/23378773/a9c41ca6-fd34-11e6-9a07-6a69a2a702ff.png)
<hr>


## API DEPLOYMENT

What I did is create two remotes, one for **Staging** and the another for **Production**. Instead of pushing directly, I created a branch and pushed to my staging remote and did the same for production, pushing to the master branch of heroku. 
No error's thrown or gems tossed (which happens a lot in windows), just warnings which I'll handle momentarily. 

![screen shot 2017-02-27 at 21 03 31](https://cloud.githubusercontent.com/assets/20464709/23377605/f470d9be-fd30-11e6-8e42-33860b559e99.png)
![screen shot 2017-02-27 at 21 04 00](https://cloud.githubusercontent.com/assets/20464709/23377606/f60dff9a-fd30-11e6-8a94-08b06f8bc8c9.png)
<hr>

Tried accessing the staged app remotely, but it returned an error. Sometimes this happens when there is nothing on the View, so I took the liberty of creating one, and pushing once again. 
![screen shot 2017-02-27 at 22 17 25](https://cloud.githubusercontent.com/assets/20464709/23381616/69030e38-fd3f-11e6-87c4-04e325212aa2.png)
<hr>

Tried accessing the url again. Production instance works remotely, staged instance fails to comply. Decided to access the logs..... Realised that heroku compiler is having issues with the router when issuing GET request. :-( 
![screen shot 2017-02-27 at 22 48 40](https://cloud.githubusercontent.com/assets/20464709/23381618/6a28497c-fd3f-11e6-9ed7-94bf71a2ed02.png)
<hr>

Hit Stackoverflow and after reading some posts, I did migrations of the DB to heroku servers. I still had errors thrown at me until I realized that Fastweb Italia was blocking some ports (5000 etc) truncating data transfer to heroku. 

I ran a detached DB migrations in heroku then re-created new Mongo DB's instances in Mlab as I had tossed the previous ones, subsequent to which I linked them to my API, deployed and life was good once again. 

![screen shot 2017-03-02 at 22 32 01](https://cloud.githubusercontent.com/assets/20464709/23528162/1deac98e-ff99-11e6-85bc-22762b67184c.png)
<hr> 









