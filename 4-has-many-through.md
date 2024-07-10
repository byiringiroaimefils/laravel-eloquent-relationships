# One to Many

We have User, Post, and Organization table. Each User can have multiple Posts, a Post should belongs to a single User, and User can have a single Organization.

## User Table

| id | name | organization_id |
| --- | --- | --- |
| 1 | Edward | 2 |
| 2 | Ed | 1 |
| 3 | Ward | 2 |

## Post Table

| id | title | body | user_id |
| --- | --- | --- | --- |
| 1 | first blog post | the first blog post i've ever made  | 1 |
| 2 | second blog post | the second blog post i've made | 1 |
| 3 | is it a blog post? | writing blog post is hard, so i wrote this instead | 3 |

## Organization Table

| id | name |
| --- | --- |
| 1 | Leaf Corp |
| 2 | Ball Foundation |
| 3 | Road Service |

# Implementation

```php
// \App\Models\User.php
class User extends Model {
    /**
     * Get posts data of a User
     */
    public function posts()
    {
        $this->hasMany(\App\Models\Post::class);
    }

    /**
     * Get organization data of a User
     */
    public function organization()
    {
        $this->hasOne(\App\Models\Organization::class);
    }
}

// \App\Models\Post.php
class Post extends Model {
    /**
     * Return the User for the Post
     */
    public function user()
    {
        $this->belongsTo(\App\Models\User::class);
    }
}

// \App\Models\Organization.php
class Organization extends Model {
    /**
     * Return the posts associated with the Organization through User data
     */
    public function posts()
    {
        $this->hasManyThrough(\App\Models\Post::class, \App\Models\User::class);
    }
}

// Usage Example
$ball_foundation_org = Organization::where('name', 'Ball Foundation')->first();
$ball_foundation_org_posts = $ball_foundation_org->posts; // posts with user_id 1 or 3, which titles are "first blog post", "second blog post", "is it a blog post?"
```