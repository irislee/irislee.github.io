---
layout: post
title: "8 Steps to Create a Simple Capybara Test"
date: 2013-11-01 17:32
comments: true
categories: 
---
What's Capybara?

{% img http://i.imgur.com/q4ZZRfa.jpg %}

Capybara is basically a library that helps you test web applications by simulating how a real user would interact with your app.

<a href="https://github.com/jnicklas/capybara">Capybara</a> requires Ruby 1.9.3 or later. To install, type in your terminal:

```
gem install capybara
```

Capybara tests are often called feature tests or end-to-end testing. It was built on top of Nokogiri in order to discover the elements on a page using HTML and CSS. It is a DSL (Domain Specific Language) that is built on top of <a href="https://github.com/rspec/rspec"RSpec</a> and <a href="http://cukes.info/">Cucumber</a>.

Capybara tests are like RSpec tests with Capybara flavoring (yum giant rodent). Some keywords:
<table border="1">
  <tr>
    <th>Capybara DSL</th>
    <th>RSpec DSL</th>
  </tr>
  <tr>
    <td>feature</td>
    <td>describe</td>
  </tr>
  <tr>
    <td>background </td>
    <td>before</td>
  </tr>
  <tr>
    <td>scenario</td>
    <td>it</td>
  </tr>
  <tr>
    <td>given</td>
    <td>let</td>
  </tr>
</table>

Capybara uses a web driver (in this example Selenium) to do some browser magic. It lets you control the browser through code and has the browser take actions on your behalf in order to make sure the website actually works as the user will experience. Normally a “headless browser” is used to save memory, but the example below will show it to you live…for funsies.

Examples of Capybara syntax:

<table border="1">
  <tr>
    <th>Method</th>
    <th>What it does</th>
  </tr>
  <tr>
    <td>visit &#8216;url&#8217;</td>
    <td>goes to the given url</td>
  </tr>
  <tr>
    <td>fill_in(locator, :with => option)</td>
    <td>finds text field by id and fills in with option</td>
  </tr>
  <tr>
    <td>click_button(locator)</td>
    <td>finds button by id and clicks it</td>
  </tr>
</table>

<h2>Capybara with Google and Youtube Searches.</h2>
<em>You can download this code <a href="https://github.com/irislee/try-capybara">here</a>.</em>

<h3>Step 1</h3>
In your project folder, add Capybara and Selenium-WebDriver to your Gemfile. It should look like this:

```ruby
source 'https://rubygems.org'

gem "sinatra"

group :test do
  gem "rspec"
  gem "capybara"
  gem 'selenium-webdriver'
end
```

<h3>Step 2</h3>
Don't forget to bundle

```
bundle install
```
<h3>Step 3</h3>
Create a simple_app.rb file (it got mad at me if I didn’t include this file).

```ruby
require_relative './environment'

get '/' do
  "hello capybara"
end
```

<h3>Step 4</h3>
<em>Normally, there’d be more folders in your application, but this is a very simple example so everything is in the top level.</em>

Your environment.rb should look like this:

```ruby
require 'bundler/setup'
Bundler.require

require './simple_app'
```

<h3>Step 5</h3>
And your config.ru…

```ruby
require './environment'

run SimpleApp
```

<h3>Step 6</h3>
Require Capybara in your spec_helper.rb file:

```ruby
require 'capybara'
include Capybara::DSL
Capybara.default_driver = :selenium

def app
  Sinatra::Application
end

set :environment, :test
RSpec.configure do |config|
  config.treat_symbols_as_metadata_keys_with_true_values = true
  config.run_all_when_everything_filtered = true
  config.filter_run :focus
  config.order = 'random'
end
```

<h3>Step 7</h3>
Here’s what my features_spec.rb looks like. I’m using Capybara to search Google.com for “Flatiron School” and then visiting YouTube to look for cat videos. In each describe block, I tell Capybara to visit the website and fill in the text field with what I’m searching for and then click submit. The `sleep 2` is there to slow down the process so you can see it better in Step 8.

```ruby
require_relative '../simple_app.rb'
require 'spec_helper.rb'

describe 'google search page', :js => true do
  it "should search the google for something" do
    visit 'http://google.com'
    fill_in("gbqfq", :with => "Flatiron School")
    sleep 2
    click_button("gbqfb")
    sleep 2
  end
end

describe 'youtube search page', :js => true do
  it "should search youtube for maru" do
    visit 'http://www.youtube.com/'
    fill_in("masthead-search-term", :with => "mugumogu")
    sleep 2
    click_button("search-btn")
    sleep 2
    page.should have_content("maru")
  end
end
```

<h3>Step 8</h3>
Now run `rspec` in your terminal and watch it do the searches and pass the tests.