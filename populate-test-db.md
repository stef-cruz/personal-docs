To be able to run specs in the container you need to populate or update the injury_profiler_test in case there are new tables in the schema.

- To do that, connect to the medinah console:   

`docker exec -it kitman-lde-medinah /bin/bash`

- Go to file `spec/rails_helper.rb` and uncomment line 104:   

`ActiveRecord::Migration.maintain_test_schema!`

- Run one of the specs in the container:   

e.g. `bundle exec rspec spec/controllers/heartbeat_controller_spec.rb`

- Comment out the line in the file `spec/rails_helper.rb` again.

- Check sequelPro and see that the injury_profiler_test schema is updated. Proceed to run your tests.
