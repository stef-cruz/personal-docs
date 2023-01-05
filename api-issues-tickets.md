## API intermittent issues

This is a general guideline on how to investigate a ticket in which the user is experiencing intermittent issues with the API calls.

The goal with this type of ticket is to try to replicate the issue locally.

#### Test locally

1. Get the API credentials. It can be in the AWS console but as I don't think I have access to it I ask around. Gustavo is a good source of knowledge. 
2. Set the creds in the rbenv.vars in Fenway

e.g. 
FIRSTBEAT_SHARED_SECRET="xxx"  
FIRSTBEAT_API_KEY="xxx"  
FIRSTBEAT_CONSUMER_ID="xxx"  

3. In console local, create the integration with the org that the user is seeing the error. You might need to ask PS (Platform Success) team to give 
you access to the org or this info might be in the ticket.

Example for Firstbeat: it needs the account_id and the team_id, and this info was in the ticket.

4. In IP, try to call the API and check the fenway console for errors.

### Other ideas:

#### Test in postman

Call the endpoint with the creds and see if there is any error.

#### Test in staging

In this case you would use the network tab in chrome dev tools to check for errors.

#### Test in rails console

Instantiate the client with the creds and throw in a binding.pry in the code to see if you can replicate the error.
