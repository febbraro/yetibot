# yetibot

yetibot is a chat bot built on the [Hubot][hubot] framework. It was
initially generated by [generator-hubot][generator-hubot].

This README is intended to help get you started. 

[hubot]: http://hubot.github.com
[generator-hubot]: https://github.com/github/generator-hubot

### Running yetibot Locally

You can test your hubot by running the following, however some plugins will not
behave as expected unless the [environment variables](#configuration) they rely
upon have been set.

You can start yetibot locally by running:

    % docker-compose run hubot

You'll see some start up output and a prompt:

    yetibot>

Then you can interact with yetibot by typing `yetibot help`.

    yetibot> yetibot help
    yetibot animate me <query> - The same thing as `image me`, except adds [snip]
    yetibot help - Displays all of the help commands that yetibot knows about.
    ...

### Configuration

A few scripts (including some installed by default) require environment
variables to be set as a simple form of configuration.

Each script should have a commented header which contains a "Configuration"
section that explains which values it requires to be placed in which variable.
When you have lots of scripts installed this process can be quite labour
intensive. The following shell command can be used as a stop gap until an
easier way to do this has been implemented.

    grep -o 'hubot-[a-z0-9_-]\+' external-scripts.json | \
      xargs -n1 -I {} sh -c 'sed -n "/^# Configuration/,/^#$/ s/^/{} /p" \
          $(find node_modules/{}/ -name "*.coffee")' | \
        awk -F '#' '{ printf "%-25s %s\n", $1, $2 }'

How to set environment variables will be specific to your operating system.
Rather than recreate the various methods and best practices in achieving this,
it's suggested that you search for a dedicated guide focused on your OS.

### Scripting

An example script is included at `scripts/example.coffee`, so check it out to
get started, along with the [Scripting Guide][scripting-docs].

For many common tasks, there's a good chance someone has already one to do just
the thing.

[scripting-docs]: https://github.com/github/hubot/blob/master/docs/scripting.md

### external-scripts

There will inevitably be functionality that everyone will want. Instead of
writing it yourself, you can use existing plugins.

Hubot is able to load plugins from third-party `npm` packages. This is the
recommended way to add functionality to your hubot. You can get a list of
available hubot plugins on [npmjs.com][npmjs] or by using `npm search`:

    % npm search hubot-scripts panda
    NAME             DESCRIPTION                        AUTHOR DATE       VERSION KEYWORDS
    hubot-pandapanda a hubot script for panda responses =missu 2014-11-30 0.9.2   hubot hubot-scripts panda
    ...


To use a package, check the package's documentation, but in general it is:

1. Use `npm install --save` to add the package to `package.json` and install it
2. Add the package name to `external-scripts.json` as a double quoted string

You can review `external-scripts.json` to see what is included by default.

##### Advanced Usage

It is also possible to define `external-scripts.json` as an object to
explicitly specify which scripts from a package should be included. The example
below, for example, will only activate two of the six available scripts inside
the `hubot-fun` plugin, but all four of those in `hubot-auto-deploy`.

```json
{
  "hubot-fun": [
    "crazy",
    "thanks"
  ],
  "hubot-auto-deploy": "*"
}
```

**Be aware that not all plugins support this usage and will typically fallback
to including all scripts.**

[npmjs]: https://www.npmjs.com

##  Persistence

We use the `hubot-s3-brain` package to save the brain as a JSON file to AWS S3.
To configure the brain you need to provide configuration via the environment 
variables defined on the [Hubut S3 Brain](https://www.npmjs.com/package/hubot-s3-brain) 
npm page.

The brain script is configured in the `external-scripts.json` file.

## Adapters

Adapters are the interface to the service you want your hubot to run on, such
as Campfire or IRC. There are a number of third party adapters that the
community have contributed. Check [Hubot Adapters][hubot-adapters] for the
available ones.

If you would like to run a non-Campfire or shell adapter you will need to add
the adapter package as a dependency to the `package.json` file in the
`dependencies` section.

Once you've added the dependency with `npm install --save` to install it you
can then run hubot with the adapter.

    % bin/hubot -a <adapter>

Where `<adapter>` is the name of your adapter without the `hubot-` prefix.

[hubot-adapters]: https://github.com/github/hubot/blob/master/docs/adapters.md

Yetibot runs with the Slack adapter

## Deployment

Yetibot is deployed to as a Docker container using the [febbraro/yetibot](https://hub.docker.com/r/febbraro/yetibot/) 
Docker Image. It is deployed on an EC2 instance. To connect to Slack you need to
provide an API key using the `HUBOT_SLACK_TOKEN` variable as detailed in the
[Slack SDK for Hubot](https://slackapi.github.io/hubot-slack/) documentation.