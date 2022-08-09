# How to test local gems in a docker container (e.g. fenway, console)

1. Add volume to LDE container - IMPORTANT: use the gem name as per gemspec, not a random name.  
`kitman lde volume add fenway --name gem-name --path /Users/stefaniecruz/dev/gem-name`

2. Make sure that path was added:  
`cd dev/kitman && bundle exec kitman lde volume show fenway`.  
Expect to see:   
`gem-name: /Users/stefaniecruz/dev/gem-name`

3. Add path to local in the fenway project Gemfile pointing to opt folder inside the container - IMPORTANT: use the gem name as per gemspec, not a random name.   
`gem('gem-name', path: '/opt/gem-name')`

4. Start LDE container.  
`kitman lde start`

5. Open another terminal window, connect to fenway docker.  
`docker exec -it kitman-lde-fenway /bin/bash`

6. Run bundle update and make sure your gem is listed with the local path.  
`bundle update`

7. Fire up the rails console.  
`bundle exec rails c`

8. Load necessary files.   
`load '/opt/petfinder/lib/petfinder/client.rb'`

9. Try to instantiate your gem’s client.  
`client = ::Kitman::Integrations::Fitbit.new`

Note: if there are required files in your gem's client.rb file, make sure to use require_relative or else the rails console won't find them and it will throw a generic NameError. So instead of using `require('./lib/kitman/zonein/constants'` at the top of your client.rb, give preference to `require_relative 'constants'`. To troubleshoot this, try and load the file e.g. `load '/opt/petfinder/lib/petfinder/client.rb'` and see the error that comes up. 

- Remove volume when done.   
`bundle exec kitman lde volume remove fenway --name gem-name`

More info [here](https://github.com/KitmanLabs/kitman/blob/1-0-stable/docs/modules/lde.md).

### Testing gems locally using the `kitman-integrations` gem:

The integration of a 3rd party source to console/medinah follows the pattern: Ruby Gem API wrapper <-> kitman-integrations <-> console/fenway/medinah.

If you're developing an API wrapper gem and want to integrate it to kitman-integrations to test in a container console or fenway, for example, you need to follow the steps:

1. In kitman-integrations, add your local gem to Gemfile and run `bundle-update`. 
`gem('petfinder', path: '/Users/stefaniecruz/dev/playground/petfinder')`.  

2. Create the relevant files to communicate with your local gem inside integrations and adapters folders.

3. Follow the steps above for both kitman-integrations and your local gem. Point both to local in the containter's Gemfile, add them to volume, load the files from both gems, instantiate the client for both too.

`load '/opt/petfinder/lib/petfinder/client.rb'`
`load '/opt/kitman-integrations/lib/kitman/integrations/adapters/petfinder.rb'`

`client = Kitman::Integrations::Adapters::Petfinder.new(client_id: 'yyy', client_secret: 'xxx')`
`Petfinder::Client.new(client_id: 'yyy', client_secret: 'xxx')`
