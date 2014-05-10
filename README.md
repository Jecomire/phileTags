PhileTags
========

Plugin adds page tagging functionality to [Phile](http://philecms.github.io/Phile).
Based on [Pico Tags by Szymon Kaliski](https://github.com/szymonkaliski/Pico-Tags-Plugin), but only uses the provided hooks
and leaves the Phile core alone.

It gives you access to:
* If on a `/tag/` URL, the `current_tag` variable
* For every page, `page.meta.tags_array` --- array of tags for the `page`

## Installation

* Place this repo into the `plugins` directory:

```bash
git clone https://github.com/pschmitt/phileTags /srv/http/plugins/phileTags 
```

* * *

* **Important:** Make a new template called `tag` (i.e. `phile/themes/<your_theme>/tag.html`) which will be used when requesting a `tag/` URI.

* * *

* Activate it in `philecms/config.php` file, by adding a line:

```php
$config['plugins']['phileTags'] = array('active' => true);
```


## Usage

Add a new `Tags` attribute to the page meta:

```
/*
Title: My First Blog Post
Description: It's a blog post about javascript and php
Author: Dan Reeves
Robots: index,follow
Date: 2014/04/04
Tags: js, javascript, php
*/
```


## Configuration

In `philecms/plugins/phileTags/config.php` you can customize:

* `$config['tag_template']` -- which template should be used for `site.com/tag/sometag` pages. 
This setting defaults to `'tag'` (mean `tag.html`).

* `$config['tag_separator']` -- the separator used for splitting the tag meta (regexps, like `'\s*'` are allowed). 
Its default value is `','`.


## Templates

You can now access both the current page `meta.tags_array` and each `page.meta.tags_array` in the `pages` array:

`tag.html` template may look like:

```html
<!DOCTYPE html>
<head>
	<title>{{ meta.title }}</title>
</head>
<body>
	<h2>Posts tagged #{{ current_tag }}:</h2>

	{% for page in pages %}
	{% if page.meta.tags_array and current_tag in page.meta.tags_array %}

		<div class="post">

			<h2><a href="{{ base_url }}/{{ page.url }}">{{ page.meta.title }}</a></h2>
			<div class="excerpt">{{ page.content }}</div>

			<span class="meta-tags">Tags:
			{% for tag in page.meta.tags_array %}
				<a href="{{ base_url }}/tag/{{ tag }}">#{{ tag }}</a>
			{% endfor %}
			</span>

		</div> <!-- // post -->

	{% endif %}
	{% endfor %}

</body>
</html>
```

And you should modify your main page's template (i.e. `themes/default/index.html`)
in page loop section, like:

```html
{% for page in pages %}

	<h3>{{ page.title }}</h3>
	{{ page.content|slice(1,300)|striptags }}

	{% if page.meta.tags_array %}
		<br><b>Tags:</b>
		{% for tag in page.meta.tags_array %}
			<a href="{{ base_url }}/tag/{{ tag }}">#{{ tag }}</a>
		{% endfor %}
	{% endif %}

{% endfor %}
```

## License

MIT
