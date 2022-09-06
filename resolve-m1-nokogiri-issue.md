## MacOs (M1 Arm64) unable to load Nokogiri 

If you face any issues with the nokogiri gem when running the command `bundle install` in a specific project (e.g. medinah, kitman-migrations, etc), run the command below specifying the architecture:

`
arch -x86_64 gem install nokogiri -v 'VERSION' --platform=ruby -- --use-system-libraries
`
