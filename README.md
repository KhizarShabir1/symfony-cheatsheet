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
