[![Build Status](https://secure.travis-ci.org/Druwerd/hudson-remote-api.png)](http://travis-ci.org/Druwerd/hudson-remote-api)
# hudson-remote-api
hudson-remote-api is ruby library to talk to Hudson's xml remote access api

## Installation:

    gem install hudson-remote-api

## Configuration:

```ruby
require 'hudson-remote-api'
```

```ruby
# Auto Configuration sets Hudson[:url]
Hudson.auto_config
```
Or

```ruby
# Manual Configuration
Hudson[:url] = 'http://localhost:8080'
Hudson[:user] = 'hudson'
Hudson[:password] = 'password'

# To turn off checking for crumbIssuer
Hudson[:crumb] = false
```
## Usage:

### List jobs
```ruby
# list all jobs
Hudson::Job.list

# list current active jobs
Hudson::Job.list_active
```

### Create (or load existing) job
```ruby
j = Hudson::Job.new('my_new_job')
```

### Actions on job
```ruby
j = Hudson::Job.new('my_new_job')

# start a build
j.build

# disable the job
j.disable

# enable the job
j.enable

# clear out the job's workspace
j.wipe_out_workspace

# delete the job
j.delete
```

### Print the last build number of a job
```ruby
j = Hudson::Job.new('jobname')
puts j.last_build
```

### Use with Git
```ruby
j.repository_url = { :url => 'https://github.com/beeplove/hudson-remote-api-mkhan.git', :branch => 'origin/master' }
```

or, only to change branch

```ruby
j.repository_url = { :branch => 'origin/master' }
```

### Set build trigger
```ruby
j.triggers = { 'hudson.triggers.SCMTrigger' => '* * * * *'}
```

or, using shortcut

```ruby
j.triggers = { 'SCMTrigger' => '* * * * *', 'TimerTrigger' => '0 22 * * *'}
```

### Add or update a trigger in existing triggers
```ruby
j.triggers = j.triggers.merge({ 'hudson.triggers.TimerTrigger' => '0 22 * * *'})
```

*Avoid using shortcut form when to edit a trigger in existing triggers

### To delete existing triggers
```ruby
j.triggers = {}
```

or,

```ruby
j.triggers = nil
```

### View current trigger
```ruby
j.triggers
```

would return hash containing trigger name in key and trigger spec in value.
Example of returned hash: {"hudson.triggers.TimerTrigger"=>"0 22 * * *", "hudson.triggers.SCMTrigger"=>"* * * * *"}