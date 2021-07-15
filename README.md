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
