# __Test your skill with the Skill Simulation API and Bespoken__
## __Overview__
Recently Amazon launched, in beta, the [multi-turn conversations using the ASK CLI and SMAPI](https://developer.amazon.com/blogs/alexa/post/af4b0637-c473-4768-bdf5-cc2b56eec0d1/now-available-test-multi-turn-conversations-beta-using-the-ask-cli-and-smapi). Making it possible to test and interact with your skills from the ASK CLI without talking.

It's a great feature and here at Bespoken, we are taking advantage of it. We're using this Alexa simulation functionality within our end-to-end testing tools. Thanks to this, and because the interactions are executed using the new [SMAPI simulation API](https://developer.amazon.com/docs/smapi/skill-simulation-api.html), the time needed to execute end-to-end tests has been drastically reduced.

This sample project shows how to configure your working environment to do end-to-end testing with the Simulation feature, create simple end-to-end test scripts and run it with the Bespoken CLI.

## __Setup your environment__

### __Pre-requisites__
* You need to have an [Amazon Developer Account](https://developer.amazon.com/)
* Deploy your skill (code and interaction model)
* Enable it for testing via the [Alexa Developer Console](https://developer.amazon.com/docs/devconsole/test-your-skill.html#test-simulator)
* Get the SkillID of the voice app you want to test
* Install or update Bespoken CLI to its latest version. Run `$ npm install -g bespoken-tools`

### __Setting up ASK-CLI__
* Install ask-cli by running `$ npm install -g ask-cli`
* Initialize ask-cli by running `$ ask init`
    * Select _"No. Use the AWS environment variables"_.
    * Your web browser will be open so you can provide your Amazon Developer credentials.
    * After the authentication process has ended, return to your CLI and select the VendorID to complete the initialization process.

### __Configure your test scripts__
Using SMAPI Simulation tests requires that we have the skillId being tested, as well as the stage of development it is in.

Just add this to the configuration section of the test (or the testing.json file), like so:
```yaml
---
configuration:
 locale: en-US
 type: simulation
 skillId: amzn1.ask.skill.612ef5f8-697a-43ab-ac3c-2523a0617b7b
 stage: development
```

## __Running End-to-End Tests__
The tests can be found under the `test/simulation` folder.

Key configuration settings for the tests are found in the `testing.json`. For full details on how the `testing.json` works, read [here](https://read.bespoken.io/end-to-end/guide/#configuration).

End-to-end tests will interact with your actual Alexa skill using the Simulation feature and our simple testing scripts. Here is an example test:  
```YAML
---
- test: Invoke the skill, play once with one player and stop. 
- open guess the price: how many persons are playing today?
- one: please tell us your name
- jordi: "let's start the game: Jordi your product is * Guess the price"
- two hundred and forty: You said 240 , the actual price was * dollars. Your score for that answer is * points. Your next product is * Guess the price
- stop: Goodbye!
```
For this test, the utterances on the left-hand side will be sent to Alexa using the dialog command. The skill will respond, and we'll compare what is coming back with the expected response on the right-hand side. It's easy to create full-cycle tests using this approach, which test all aspects of the system, including the interaction model (i.e., ensuring that utterances match up correctly to the intent), Display output, AudioPlayer behavior, etc.

To run the tests, simply open a command-line terminal, go to the project directory and type:  
```BASH
$ bst test test\simulation
```

You should see output like this:
```BASH
BST: v2.1.9  Node: v8.9.4
bst test lets you have a complete set of unit tests using a simple YAML format. Find out more at https://read.bespoken.io.

 PASS  test\e2e\BasicTest.e2e.yml (42.232s)
  en-US
    Launch request, play once with one player and stop.
      √ open guess the price
      √ one
      √ jordi
      √ two hundred and forty
      √ stop
    Launch request, play once with two players and cancel.
      √ open guess the price
      √ two
      √ jordi
      √ caterina
      √ one hundred and forty nine
      √ forty three
      √ cancel

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        42.442s
Ran all test suites.
```

The results show:
* What happened with each of the tests
* Summarize the results as a whole
* Provide total time needed to complete the test execution

## __Wrapup__
To learn more about how to ensure the quality and availability of your voice apps with Bespoken Tools, take a look here:     https://bespoken.io/testing.

And if you need assistance, reach out to Bespoken on any of these channels:
* [Email](mailto:support@bespoken.io)
* [Twitter](https://twitter.com/bespokenio)
* [Gitter](https://gitter.im/bespoken)
