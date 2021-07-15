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
