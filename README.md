# symfony-cheatsheet

https://twig.symfony.com/
## Twig

### Twig-syntaxes


#### `{{ }}` Say something


#### `{% %}` Do something

```HTML
<h2>Comments</h2>
<ul>
    {% for comment in comments %}
        <li>{{ comment }}</li>
    {% endfor %}
</ul>
```

#### `{# #}` Comments

#### Filters

`<h2>Comments ({{ comments|length }})</h2>`

##### Wrapping the path in twig asset( function)

`<link rel="stylesheet" href="{{ asset('css/font-awesome.css') }}">`

## Service

Any object that does work like generating URLs.

###Logger

tail -f var/log/dev.log
### Autowiring 
If you need a service object you just need to know the correct type-hint to use.
`./bin/console debug:autowiring`
this command shows us the services we'll want to use 99% of the time.

This gives a full list of all of the type-hints that you can use to get a service.

**Bundles** put services in containers. In container services have an internal names.

**Bundles** are symfony's plugin system. Bundles prepares these service objects and put them into the container.

#### Multiline HEREDOC syntax:
        `$articleContent = <<<EOF
 
EOF;`

`index.php` is called the **Front Controller**. The first file that is executed for every page.

### `Kernel.php`
this function loads /config/bundles.php
    public function registerBundles(): iterable
    {
        $contents = require $this->getProjectDir().'/config/bundles.php';
### difference between dev and prod environments
One big difference between the dev and prod environments is that in the prod environment, the internal Symfony cache is not automatically rebuilt. That's because the prod environment is wired for speed.

In practice, this means that whenever you want to switch to the prod environment... like when deploying... you need to run a command:

`./bin/console cache:clear`

### Creating services

When you create a service class, the arguments to its constructor are autowired. That means that we can use any of the classes or interfaces from debug:autowiring as type-hints.

### % sign
```YAML
parameters:
    cache_adapter: cache.adapter.apcu
framework:
    cache:
        # APCu (not recommended with heavy random-write workloads as memory fragmentation can cause perf issues)
        app: '%cache_adapter%'
```
 whenever you surround a string with percent signs, Symfony will replace this with that parameter's value.
 
### Creating environment specific services file
Create a new config file called services_dev.yaml. This is the built-in way to create an environment-specific services file.

### Environment variables

Saving sensitive stata in `.env.local` file and showing only sentsitive but not secret data in `.env` file.
#### `nexy_slack.yaml` file

```
nexy_slack:
    endpoint: '%env(SLACK_WEBHOOK_ENDPOINT)%'
```

#### `.env.local` file ignored by git because it is added in .gitignore file
```
### CUSTOM VARS
SLACK_WEBHOOK_ENDPOINT=https://hooks.slack.com/services/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
### CUSTOM VARS END
```

#### `.env` file comiited to git (not containing sectret info)
```
### CUSTOM VARS
SLACK_WEBHOOK_ENDPOINT=https://hooks.slack.com
### CUSTOM VARS END
```

#### Environment variables in production

But if setting environment variable is tough in your situation, well, you could still use the `.env` file. I mean if we deployed right now, we could create this file, put all the real values inside, and Symfony would use that! Well, if you're planning on doing this, make sure to move the dotenv library from the require-dev section of your composer.json to require by removing and re-adding it:
```
composer remove symfony/dotenv
composer require symfony/dotenv
```

#### Casting values for environment variables

Don't worry! Environment variables have one more trick! You can cast values by prefixing the name with, for example, string::

##### `config/packages/nexy_slack.yaml`

```
nexy_slack:
    endpoint: '%env(string:SLACK_WEBHOOK_ENDPOINT)%'
```

Well, this is already a string, but you get the idea!

To show some better examples, Google for Symfony Advanced Environment Variables to find a blog post about this feature. Cooooool. This DATABASE_PORT should be an int so... we cast it! You can also use bool or float.

#### Setting Default Environment Variables

This is great... but then, the Symfony devs went crazy. First, as you'll see in this blog post, you can set default environment variable values under the parameters key. For example, by adding an env(SECRET_FILE) parameter, you've just defined a default SECRET_FILE environment value. If a real SECRET_FILE environment variable were set, it would override this.
```
parameters:
    env(SECRETS_FILE): '/etc/secure/example.com/secrets.json'
```
#### Custom Processing
More importantly, there are 5 other prefixes you can use for special processing:

First, `resolve:` will resolve parameters - the `%foo%` things - if you have them inside your environment variable;

Second, you can use `file:` to return the contents of a file, when that file's path is stored in an environment variable;

Third, `base64:` will base64_decode a value: that's handy if you have a value that contains line breaks or special characters: you can base64_encode it to make it easier to set as an environment variable;

Fourth, `constant:` allows you to read PHP constants;

And finally, `json:` will, yep, call your friend Jason on the phone. Hey Jason! I mean, it will json_decode() a string.

And, ready for the coolest part? You can chain these: like, open a file, and then decode its JSON:

`app.secrets: '%env(json:file:SECRETS_FILE)%'`
Actually, sorry, there's more! You can even create your own, custom prefix - like `blackhole:` and write your own custom processing logic.


### Setter Injection
Now let's go a step further... In SlackClient, I want to log a message. But, we already know how to do this: add a second constructor argument, type-hint it with LoggerInterface and, we're done!

But... there's another way to autowire your dependencies: setter injection. Ok, it's just a fancy-sounding word for a simple concept. Setter injection is less common than passing things through the constructor, but sometimes it makes sense for optional dependencies - like a logger. What I mean is, if a logger was not passed to this class, we could still write our code so that it works. It's not required like the Slack client.

Anyways, here's how setter injection works: create a public function setLogger() with the normal LoggerInterface $logger argument:

```
 40 lines  src/Service/SlackClient.php
use Psr\Log\LoggerInterface;
class SlackClient
{
    private $slack;
    /**
     * @var LoggerInterface|null
     */
    private $logger;
    public function __construct(Client $slack)
    {
        $this->slack = $slack;
    }
    public function setLogger(LoggerInterface $logger)
    {
        $this->logger = $logger;
    }
    public function sendMessage(string $from, string $message)
    {
        if ($this->logger) {
            $this->logger->info('Beaming a message to Slack!');
        }
  //code is missing here
    }
}
```
Create the property for this: there's no shortcut to help us this time. Inside, say $this->logger = $logger:


