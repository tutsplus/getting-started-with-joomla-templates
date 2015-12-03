
# Joomla Object Glossary

One of the most difficult things about creating templates for Joomla is finding out how to get variables and parameters for certain pages, such as a blog post's title, link, author and publish date. This file is a glossary that shows exactly that, making it easier to develop themes.

How does it work? Every file (found in the subfolders of the ``html`` folder in the Git Repo) is a template 'layout override'. And for every file, Joomla pulls variables from a variety of objects. (For a list of which files and their corresponding objects, please see the Objects Guide below).

For example, if I want to create a template for every blog post item on the blog category page, I'd have to turn something like this:
```html
<article>
  <h1>{{TITLE HERE}}</h1>
  <span>Written by {{AUTHOR}}</span>
  {{CONTENT}}
</article>
```
into:
```php
<?php $post = $this->item; ?>
<article>
  <h1><?php echo $post->title; ?></h1>
  <span>Written by <?php echo $post->author; ?></span>
  <?php echo $post->fulltext; ?>
</article>
```

And put that into the ``blog_item.php`` file. Notice in the first line, I call the ``$post`` object, required for the file to work.

Happy theming!

— [CJ Melegrito](http://mlgrto.com)


## Objects Guide

The following pages require the corresponding objects below:
- ``com_content/article/default.php`` → ``$post``, ``$page``
- ``com_content/category/blog_item.php`` → ``$post``
- ``com_content/category/blog.php`` → ``$page``, ``$category``
- ``com_content/featured/default_item.php`` → ``$post``
- ``com_content/featured/default.php`` → ``$page``


## ``$post`` Object
    $post = $this->item;

### Get Method:
    $post->parameter_name_here;

### Available Parameters:
    id
    title
    alias
    introtext
    fulltext
    created
    modified
    modified_by_name
    publish_up
    publish_down
    images
    urls
    attribs
    metadata
    hits
    featured
    category_title
    category_route
    author
    author_email
    published
    params
    tags
    text
    link

## ``$post->params`` Object
    $params = $post->params;

### Get Method:
    $params->get('parameter_name_here');

### Available Parameters:
#### Booleans:
    access-view
    access-edit
    show_title
    show_intro
    show_category
    show_parent_category
    show_author
    show_create_date
    show_modify_date
    show_publish_date
    show_readmore
    show_readmore_title
    show_icons
    show_print_icon
    show_email_icon
    show_hits


## ``$page`` Object
    $page = $this->params;

### Get Method:
    $page->get('parameter_name_here');

### Available Parameters:
#### Booleans:
    show_title
    show_intro
    show_category
    show_parent_category
    show_author
    show_create_date
    show_modify_date
    show_publish_date
    show_readmore
    show_readmore_title
    show_icons
    show_print_icon
    show_email_icon
    show_hits
    show_category_title
    show_description
    show_description_image
    show_empty_categories
    show_no_articles
    show_subcat_desc
    show_cat_num_articles
    show_pagination
    show_pagination_results
    show_feed_link
#### Strings:
    float_intro
    float_fulltext
    feed_summary
    layout_type
    page_title
    page_description
    page_rights
    page_heading
    page_subheading


## ``$category`` Object
    $category = $this->category;

### Get Method:
    $category->getParams()->get('parameter_name_here');
    $category->get('parameter_name_here');

### Available Parameters:
#### getParams():
    image
    image_alt
#### get():
    description



# Joomla Snippets

## Render Featured Posts
For ``com_content/category/blog.php``
```php
$post_featured = $this->lead_items;

foreach ($post_featured as &$item) :
  $this->item = &$item;
  echo $this->loadTemplate('item');
endforeach;
```

## Render Regular Posts
For ``com_content/category/blog.php``
```php
$post_regular = $this->intro_items;

foreach ($post_regular as &$item) :
  $this->item = &$item;
  echo $this->loadTemplate('item');
endforeach;
```

## Display Pagination
For ``com_content/category/blog.php``
```php
$number_of_pages = $this->pagination->get('pages.total');

if ($number_of_pages > 1) :
  echo $this->pagination->getPagesCounter();
  echo $this->pagination->getPagesLinks();
endif;
```

### Unique Post link
For the ``$post`` Object
```php
$post_link = JRoute::_(ContentHelperRoute::getArticleRoute($this->item->slug, $this->item->catid, $this->item->language));
```
