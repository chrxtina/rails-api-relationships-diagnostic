# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  Allows you to create a many-to-many relationship
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  Profile: give_name, surname, email
  Movies: title, release_date, length
  Favorites: profile_id, movie_id

```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :movies, inverse_of: :favorites
  belongs_to :profiles, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  A serializer is like a mirror that allows you to display only what you want to display to the view-state.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :id, :give_name, :surname, :email, :movies, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  bundle exec rails g scaffold Favorites movie:references profile:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  Dependent:destroy is a method that allows you to delete the association of one
  thing with another. For instance, if you delete a Profile, you also want to delete the Favorites that were associated with that user.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  Another example of a many-to-many relationship could be in an application for voting. One model is a Citizens and another model is Candidates. The joins table would be Votes. Citizens can have many candidates (president, senate, etc.) through their votes and Candidates can have many citizens who voted for them.

  The one-to-many relationship could be that a Candidate has many Policies. So a policies table would have a foreign key with candidate_id.
```
