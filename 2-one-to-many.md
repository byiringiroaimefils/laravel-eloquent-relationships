# One to Many

We have User and Post table. Each User can have multiple Posts, but a Post should belongs to a single User.

## User Table

| id | name |
| --- | --- |
| 1 | Edward |
| 2 | Ed |
| 3 | Ward |

## Post Table

| id | title | body | user_id |
| --- | --- | --- | --- |
| 1 | first blog post | the first blog post i've ever made  | 1 |
| 2 | second blog post | the second blog post i've made | 1 |
| 3 | is it a blog post? | writing blog post is hard, so i wrote this instead | 3 |

# Implementation

```php
// \App\Models\User.php
\App\Models
class User extends Model {
    /**
     * Get posts data of a User
     */
    public function posts()
    {
       return  $this->hasMany(Post::class);
    }
}

// \App\Models\Post.php
class Post extends Model {
    /**
     * Return the User for the Post
     */
    public function user()
    {
        return  $this->belongsTo(User::class);
    }
}

// Usage Example
$user = User::find(1); // Edward
$user_posts = $user->posts; // Collection of Posts with user_id of 1
$user_first_post = $user_posts->first(2);
$user_first_post_title = $user_first_post->title; // second blog post
```
