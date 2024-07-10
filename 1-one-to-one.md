# One to One

We have User and Verification table. Each User should only have one verification data, and each verification data should only belongs to a single User.

## User Table

| id | name |
| --- | --- |
| 1 | Edward |
| 2 | Ed |
| 3 | Ward |

## Verification Table

| id | email | is_verified | user_id |
| --- | --- | --- | --- |
| 1 | edward@mail.com | false | 1 |
| 2 | ed@mail.com | true | 2 |
| 3 | ward@mail.com | false | 3 |

# Implementation

```php
// \App\Models\User.php
class User extends Model {
    /**
     * Get verification data of a User
     */
    public function verification()
    {
        $this->hasOne(\App\Models\Verification::class);
    }
}

// \App\Models\Verification.php
class Verification extends Model {
    /**
     * Return the User for the Verification
     */
    public function user()
    {
        $this->belongsTo(\App\Models\User::class);
    }
}

// Usage Example
$user = User::find(2); // Ed
$user_verification = $user->verification;
$user_verification_email = $user->verification->email; // ed@mail.com
$is_user_verified = $user->verification->is_verified; // true

// Inverse
$verification = Verification::find(1); // email: edward@mail.com
$verification_user = $verification->user;
$verification_user_name = $verification->user->name; // Edward
```