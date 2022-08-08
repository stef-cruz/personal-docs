# How to test local gems in Fenway

- Add volume to LDE container - IMPORTANT: use the gem name as per gemspec, not a random name.  
`kitman lde volume add fenway --name gem-name --path /Users/stefaniecruz/dev/gem-name`

- Make sure that path was added:  
`cd dev/kitman && bundle exec kitman lde volume show fenway`.  
Expect to see:   
`gem-name: /Users/stefaniecruz/dev/gem-name`

- Add path to local in the fenway project Gemfile pointing to opt folder inside the container - IMPORTANT: use the gem name as per gemspec, not a random name.   
`gem('gem-name', path: '/opt/gem-name')`

- Start LDE container.  
`kitman lde start`

- Open another terminal window, connect to fenway docker.  
`docker exec -it kitman-lde-fenway /bin/bash`

- Run bundle update and make sure your gem is listed with the local path.  
`bundle update`

- Fire up the rails console.  
`bundle exec rails c`

- Try to instantiate your gem’s client.  
`client_fitbit = ::Kitman::Integrations::Fitbit.new`

Note: if there are required files, make sure the path matches the one in the /opt/ folder inside the container, or else the rails console won't find the required files that the client might need to run and throw a NameError. To troubleshoot this, try and load the file e.g. `load '/opt/petfinder/lib/petfinder/client.rb'` and then instantiate the client.

- Remove volume when done.   
`bundle exec kitman lde volume remove fenway --name gem-name`

More info [here](https://github.com/KitmanLabs/kitman/blob/1-0-stable/docs/modules/lde.md).
