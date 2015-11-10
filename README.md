# Quiz #3

## Instructions

1. Fork this repo
2. Clone your fork
3. Fill in your answers by writing in the appropriate area, or placing an 'x' in
the square brackets (for multiple-choice questions).
4. Add/Commit/Push your changes to Github.
5. Open a pull request.

**Note**: only place your answer between backticks. Don't modify the backticks,
or the language specifier after them.

## Ruby Basics & Enumerables (meets Beauty and the Beast)


### Question 1

Define a method called `offerRose`, which should take one argument named person.

When called, the method should `puts`, "Would you take this rose and help out
an old beggar, X?", where X is the person passed into the method.

Demonstrate calling the method with an argument of "young prince".

Write your code here:
```ruby
def offer_rose( person )
  puts "Would you take this rose and help out an old beggar, #{person}?"
end
```

### Question 2

Assume the following hash:

```ruby
town = {
  residents: ["Maurice", "Belle", "Gaston"],
  castle: {
    num_rooms: 47,
    residents: "Robby Benson",
    guests: []
  }
}
```

Using Ruby, remove Belle from the town residents, and
add her to the list of guests in the castle.

Write your code here:
```ruby
# one line solution
town[:castle][:guests] << town[:residents].delete( "Belle" )

# multi-line solution
belle = town[:residents][1]
town[:residents].delete( belle )
town[:castle][:guests] << belle
```

### Question 3

Assume you have an array of strings representing friend's names:

```ruby
friends = ["Chip Potts", "Cogsworth", "Lumière", "Mrs. Potts"]
```

Using `.each` AND string interpolation, produce output (using `puts`) like so:

```
Belle is friends with Chip Potts
Belle is friends with Cogsworth
Belle is friends with Lumière
Belle is friends with Mrs. Potts
```

Write your code here:
```ruby
friends.each do |friend|
  puts "Belle is friends with #{friend}"
end
```

## SQL, Databases, and ActiveRecord (meets Aladdin)

### Question 4

Describe what an ERD is, and why we create them for applications. Also give an
example what the attributes and relationships might be for the following
entities (no need to draw an ERD):
<!-- Maybe clarify whether they're meant to give relationships between all four entities or... -->
* Genie
* Lamp
* Person
* Pet

Your answer:
```
An ERD (Entity Relationship Diagram) is a graphical representation of data that portrays the attributes of different entities and the relationships between them. Each entity in an ERD represents a database table.

Relationships
* Genie, Lamp: one-to-one
* Person, Genie: one-to-one
* Person, Pet: one-to-many

Attributes
* Genie: name, wishes, person_id
* Lamp: location, genie_id, person_id
* Person: name, age, address
* Pet: name, age, species, person_id

```

### Question 5

Describe what a schema is, and how we represent a one-to-many relationship in a
SQL database. If you need an example, you can use: people and wishes
(one-to-many).

Your answer:

```
A schema defines the columns that comprise the tables in our database. Each attribute in a schema (e.g, name TEXT) corresponds to a column in a table.

We represent one-to-many relationships in schema using foreign keys. The table/class that there are "many" of has a column for this foreign key.

CREATE TABLE people(
  id SERIAL PRIMARY KEY,
  name TEXT,
  age INTEGER,
);

CREATE TABLE wishes(
  id SERIAL PRIMARY KEY,
  wish_action TEXT,
  wish_date TEXT,
  people_id INTEGER
);
```

### Question 6

**Assume:**
1. Your database two working tables, `genies` and `lamps`.
2. You have a working connection to the database for ActiveRecord.
3. You have active record models defined for `Genie` and `Lamp`, and the
relationships between the two are set up in Active Record.
4. Lamps have one property, `wishes_remaining`, and genies have one property, `name`.

Write code to do the following:

1. Create a lamp with 3 wishes remaining and a genie named 'Genie'
2. Create a relationship between 'Genie' and the lamp.
3. Update the lamp so it only has one wish left.
  * Oh no... Jafar has Aladdin! Thankfully he's wished to become a genie himself!
4. Create a new Genie named 'Jafar' and put him in a new lamp with 3 wishes left.
5. Free the good Genie by setting his lamp to `nil`


Write your code here:
```ruby
# 1
# Using the bang symbol will cause .create to throw an error if our genie or lamp do not pass validations.
lamp = Lamp.create!( wishes_remaining: 3 )
genie = Genie.create!( name: "Genie" )

# 2
lamp.genie = genie
lamp.save

# 3
lamp.update!( wishes_remaining: 1 )

# 4
jafar = Genie.create!( name: "Jafar" )
jafars_lamp = Lamp.create!( wishes_remaining: 3 )
jafars_lamp.genie = jafar
jafars_lamp.save

# 5
genie.lamp = nil
  # or
genie.lamp.destroy
```

## Sinatra / REST (meets Mulan)

### Question 7

The Chinese Emperor needs an application to help him manage his warriors.

Describe to him what a RESTful route is, and list what the seven RESTful routes
would look like for such an application.

Your description:
```
A RESTful (REpresentational State Transfer) route is a method and path combination that corresponds to a CRUD function (create, update, read or destroy). REST is a convention that most developers follow so that communication between browsers and servers is standardized.
```
Your routes:
```
GET '/warriors'
  * Index

GET '/warriors/new'
  * New warrior form

POST '/warriors'
  * Create warrior

GET '/warriors/:id'
  * This is the show route, which finds a warrior by ID, and displays information about that warrior.

GET '/warriors/:id/edit'
  * Edit warrior form

PATCH '/warriors/:id'
  * Update warrior

DELETE '/warriors/:id'
  * Delete warrior
```

### Question 8

Assume:
* Warrior is an ActiveRecord model, with a 'name' attribute.
* You have a Sinatra `app.rb` (or similar file), that defines the following
route:

```ruby
get '/warriors' do
  @warriors = Warrior.all
  erb :"warriors/index"
end
```

Write what an example ERB file (aka view) might look like to list all the warriors:

Write your code here (**NOTE: syntax highlighting doesn't work for ERB in markdown files, so ignore the colors!**):

```html
<ul>
  <% @warriors.each do |warrior|%>
    <li><%=warrior.name%></li>
  <%end%>
</ul>
```
