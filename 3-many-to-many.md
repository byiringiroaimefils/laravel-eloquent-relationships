# Many to Many

We have Post and Category table. Each Post can have multiple Categories, and a Categorie can associated with multiple Posts.

## Post Table (post)

| id | title | body | user_id |
| --- | --- | --- | --- |
| 1 | first blog post | the first blog post i've ever made  | 1 |
| 2 | second blog post | the second blog post i've made | 1 |
| 3 | is it a blog post? | writing blog post is hard, so i wrote this instead | 3 |

## Category Table (category)

| id | title |
| --- | --- |
| 1 | sport |
| 2 | news |
| 3 | politic |

## Pivot table (post_category)

| id | post_id | category_id |
| --- | --- | --- |
| 1 | 1 | 1 |
| 2 | 1 | 2 |
| 3 | 3 | 1 |

# Implementation

```php
// \App\Models\Post.php
class Post extends Model {
    /**
     * Return the categories of the Post
     */
    public function categories()
    {
        $this->belongsToMany(\App\Models\Category::class);
    }
}

// \App\Models\Category.php
class Category extends Model {
    /**
     * return the posts associated with the Category
     */
    public function posts()
    {
        $this->belongsToMany(\App\Models\Post::class);
    }
}

// Usage Example
$post = Post::find(1);
$post_categoris = $post->categories; // sport, news

$sport_category = Category::where('title', 'sport')->first();
$sport_category_posts = $sport_category->posts; // post_id: 1, 3
```