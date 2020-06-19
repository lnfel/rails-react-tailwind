## Rails with React + Tailwindcss

An ambitious crossover that nobody asked for. Feel free to clone and test the repo:

    git clone https://github.com/lnfel/rails-react-tailwind.git

see live sample at [Heroku](https://rails-react-tailwind.herokuapp.com/)

---

## Steps to re-create the project

**Create new rails app:**

    $ rails new my-app
    $ cd my-app

**Add webpacker and react-rails to your gemfile:**

    gem 'webpacker'
    gem 'react-rails'

Note: on Rails 6.x webpacker is already included.

**Now run the installers:**

    $ bundle install
    $ rails webpacker:install
    $ rails webpacker:install:react
    $ rails generate react:install

**Generate your first component:**

    $ rails g react:component HelloWorld greeting:string

**Generate controller and a view:**

    $ rails generate controller Site index

**Render it in a Rails view:**

`<!-- app/views/site/index.html.erb -->`

    <%= react_component("HelloWorld", { greeting: "Hello from react-rails." }) %>

**Set route:**

`<!-- config/routes.rb -->`

    root 'site#index'

**Let's start the app:**

    $ rails s

Output: greeting: "Hello from react-rails", inspect webpage in your browser too see change in tag props.

**Make the app production ready:**

move sqlite3 gem inside development and test group

`<!-- Gemfile -->`

    group :development, :test do
      # Call 'byebug' anywhere in the code to stop execution and get a debugger console
      gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
      # Put sqlite3 here
      gem 'sqlite3', '1.4.2'
    end

then add pg gem

    gem 'pg'

change production settings

`<!-- config/database.yml -->`

    production:
     adapter: postgresql
     database_url: <%= ENV['DATABASE_URL'] %>
     secret_key_base: <%= ENV['SECRET_KEY_BASE'] %>
     rails_master_key: <%= ENV['RAILS_MASTER_KEY'] %>
     encoding: unicode
     pool: 5

Push the repo to Github and deploy the app to Heroku.

If you only need react for rails, you're good to go.
To integrate tailwind for your app, follow the next steps

## Integrating Tailwincss

**Intall tailwind using yarn:**

    $ yarn add tailwindcss

**Include tailwindcss in postcss.config.js:**

`<!-- postcss.config.js -->`

    module.exports = {
     plugins: [
       // ...
       require('tailwindcss')('./app/javascript/stylesheets/tailwind.js'),
       require('autoprefixer'),
       // ...
     ]
    }

**Setup tailwind for rails:**

Create a folder named **"stylesheets"** in app/javascript
then create **"application.scss"** inside stylesheets folder

`<!-- app/javascript/stylesheets/application.scss -->`

    @import "tailwindcss/base";
    @import "tailwindcss/components";
    @import "tailwindcss/utilities";

**Load application.scss:**

`<!-- app/javascript/packs/application.js -->`

    require("stylesheets/application.scss")

**Change stylesheet_link_tag to stylesheet_pack_tag**

`<!-- app/views/layouts/application.html.erb -->`

    <%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>

Create a file named **"tailwind.config.js"** in app/javascript/stylesheets

then paste this code:

    module.exports = {
      theme: {
        extend: {}
      },
      variants: {},
      plugins: []
    }

Try using tailwind utility classes on app/views/site/index.html.erb

**Finally run the app:**

    $ rails s

Push the repo to Github and deploy the app to Heroku.
