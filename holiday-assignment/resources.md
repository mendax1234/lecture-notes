# Resources

## RESTful APIs

Representational State Transfer, REST in short, is an architecture for networked applications. Being one of the simplest architectures to deploy, it is a popular choice for many web services. It is a client-server architecture where a client initiates a request to the server to be processed, and receives a response with updated data. The Application Programming Interface or API defines the specifications of this communication. In the context of this assignment, the server will be our Go or Ruby on Rails application, while the client will be our React application.

Requests and responses can take many forms - JSON is the most common format. JavaScript Object Notation or JSON is a text-based data-interchange format. It is already supported by all modern browsers and server-side scripting languages so you do not need to implement it yourself, but if you want to know more, you can visit [this website](https://www.json.org/json-en.html). You may also want to read up on [HTTP Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages), which will be how the JSON data are exchanged.

Note that this is just one of the many ways that a frontend can interface with a backend. If you wish to try this out, do check out [this guide](https://guides.rubyonrails.org/api_app.html) for Ruby on Rails and [this guide](https://reactjs.org/docs/faq-ajax.html) for React.

### HTTP Messages

## Relational Database

Relational databases are databases that store data in tables, each containing a number of columns and rows. It is called ”relational” because tables are linked to other tables through foreign keys. For example, the Singapore Government’s address database may use the NRIC/FIN of individuals to link addresses in the `addresses` table to names in the `names` table. In this case, the column containing NRIC/FIN in the `addresses` table is called the foreign key as it references the primary key in the `names` table.

In this assignment, we will be going through fundamental database concepts. Usually, an application will use one database, with several applications sharing the same database server. For instance, if you and a friend each had a blog, each blog needs one database, but the same server could have two databases defined, one for each blog.

At the highest level, a database contains a schema. A schema is a blueprint of your tables, containing their structures and relationships.

{% hint style="info" %}
I would say this part is quite similar to CS50's Week 7 content about SQL. You can find more information at [Week 7 - SQL](https://app.gitbook.com/s/6nZ3LlEi4QjmAU8BqKIP/lec-sec-probset/week-7-sql "mention").
{% endhint %}

