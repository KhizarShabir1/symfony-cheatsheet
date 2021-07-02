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


