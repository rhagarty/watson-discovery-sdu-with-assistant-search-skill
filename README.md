# watson-discovery-sdu-with-assistant-search-skill

* Use Assistant dialog for better chatbot experience
* Use "customer-care" skill provided by Assistant
* Use search skill to search Disco if question is "anything-else" and reply with disco passages

# What is a Search Skill?

PLUS or PREMIUM PLAN REQUIRED (CURRENTLY BETA)

You can have a search skill by itself, where all user input is passed to discovery.

If you have both, only if the dialog skill reaches "anything_else" node will it search skill be triggered.

# Differences between Search Skill and Webhooks

* No code required inside dialog node
* Configuration of combinging dialog skill and search skill is done by creating a "Assistant" instance.
* Use of search skill requires using the V2 version of the Assistant Service API.

# TODO:

* Remove references to calling Disco directly
* Add action creds to .env
* Create better use case for invoking search skill from Assistant dialog node
* Format Disco results better in chatbot
* Modify search type in Discovery to use passages
* Integrate into Slack
* Push app to cloud

# Prerequisites

* Create Watson Discovery collection as described in https://github.com/rhagarty/watson-discovery-sdu-ui
* Create Watson Assistant dialog skill as described in  https://github.com/rhagarty/watson-discovery-sdu-with-assistant-cloud-functions

# Steps:

## Create Search Skill

Start creation process:

![](doc/source/images/select-search-skill.png)

Name your skill:

![](doc/source/images/create-search-skill.png)

Attach to Discovery instance and collection:

![](doc/source/images/assign-search-skill-to-collection.png)

Configure how the search will respond:

![](doc/source/images/configure-search-skill.png)

## Invoke the Search Skill from dialog node

Modify the "Ask about product" dialog node to call out to the search skill:

![](doc/source/images/assign-search-skill-to-node.png)

## Create Assistant instance to combine dialog and search skills:

In order to use both skills, you need to create an `Assistant` object that integrates them.

![](doc/source/images/create-assistant-to-link-skills.png)

## Test in Assistant Tooling

From the `Assistant` panel, press the `Preview Link` button:

![](doc/source/images/preview-option-button.png)

Then press the `Preview` URL:

![](doc/source/images/preview-link.png)

Then use the chatbot to test accessing the search skill:

![](doc/source/images/preview-example.png)

## Configure credentials

The credentials for your `Assistant` instance by clicking the options dropdown button located on the right side of the instance listed in the `Assistant` panel. Click the `Settings` option.

![](doc/source/images/assistant-list.png)

Click `API Details` to show all credentials:

![](doc/source/images/assistant-creds.png)

Use these values to populate your `.env` file in the next step.

```bash
cp env.sample .env
```

Edit the `.env` file with the necessary settings.

#### `env.sample:`

```bash
# Copy this file to .env and replace the credentials with
# your own before starting the app.

ASSISTANT_IAM_APIKEY=<add_assistant_iam_apikey>
ASSISTANT_ID=<add_assistant_id>

# Run locally on a non-default port (default is 3000)
# PORT=3000
```

## Run locally

```bash
npm install
npm start
```

Access the UI by pointing your browser at `localhost:3000`.

Sample questions:

* **how do I set a schedule?**
* **how do I set the temperature?**
* **how do I set the time?**

## Access to results in application

Results will be returned in Assistant response object.

* If the result is from the normal dialog conversation:

```
{ output:
   { generic:
      [ { response_type: 'text',
          text: 'Would you like me to transfer you to a representative?' } ],
     intents:
      [ { intent: 'General_Connect_to_Agent',
          confidence: 0.9979484558105469 } ],
     entities: []
    }
}
```

If the result is from the search skill:

```
{ output:
   { generic:
      [ { response_type: 'search',
          header: 'I searched my knowledge base and found this information which might be useful:',
          results:
           [ { body: 'Do you hear that? That’s the sound of hundreds of thousands of ecobee-ers welcoming you to the hive. Congratulations on the purchase of your customized web portal, visit ecobee.com and click on the Login link. To select a tile, touch or click on it. To close a tile and return back to the main screen, touch or click (top-right corner).',
               title: '["Overview"]',
               url: null },

```

# Sample Output

![](doc/source/images/sample-output.png)
