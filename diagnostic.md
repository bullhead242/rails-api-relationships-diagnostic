# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
Join tables are valuable in order to build additional tables which are dependent
  on information provided in a table which already exists, such as the information in 'Appointments' depends on the 'Doctors' and 'Patients'
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
A Profile has many favorites, while movies have many profiles and favorites belong to a profile. A favorites belong to a profile, but has many movies.
Each movie has many Profiles, and is related to a Profile through favorites.```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
  belong_to :profile, inverse_of :favorites
  has_many :movies, dependent :destroy
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
The Serializer tells our database how to arrange and store
                          the information given to it

```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :id, :surname, :email, :movies, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
rails g scaffold favorites profile:references movie:references title:string, release_date: date, length: integer
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
Dependent: Destroy is used to indicate a relationship where the dependent should be
    destroyed if the original is destroyed. ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
A WDICohort has many students, and has many consultants. Each consultant has_many students through a WDICohort yet belongs to the WDICohort. Each student has many consultants through a WDICohort, but also belongs to the WDICohort.

```
