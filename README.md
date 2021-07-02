# symfony-cheatsheet

## Twig-syntaxes

{{ }} Say something

{% %} Do something

```HTML
<h2>Comments</h2>
<ul>
    {% for comment in comments %}
        <li>{{ comment }}</li>
    {% endfor %}
</ul>
```

{# #} Comments
