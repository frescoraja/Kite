## Part 1

**Question 1:**

`What does MVC stand for and what does each part of MVC do?`

MVC stands for Model, View and Controller, and is an acronym used to define a common architectural pattern in programming. It separates the user interaction, public and private interfacing, and business logic into three classes. The model manages the data and behavior of the application, the view is the output or presentation of the data, and the controller takes input and translates it into actions to perform within the view or the model.

**Question 2:**

```ruby
# What does the following code do?
company = Company.new({name: 'Pied Piper'})
company.save
```

This code uses ActiveRecord's Object Relational Mapping of the database to instantiate the "Company" model and then commit it to the database. The `new` method, without arguments, returns an empty instance of the class it's called on. In this case, since arguments are provided, a new instance of `Company` is returned with it's `name` attribute set to 'Pied Piper'. The `save` method then performs a SQL query to commit the new model to the database, returning a boolean value based on whether the model was successfully saved to the database or rejected due to validations failing.

**Question 3:**

```javascript
// Where is the error in the following code?
// How would you fix it?
({
  baz: 'foobarbaz',
  bar: function(){
    return 'foobar';
  },
  foo: function() {
    console.log(this.baz);
    setTimeout(function() {
      console.log(this.bar());
    }, 100);
  }
}.foo());
```

Within the `foo` function, setTimeout is called with an anonymous function as the first parameter. Within that anonymous function, `this.bar` is invoked as an argument to console.log, and since JavaScript is a function-scope language, `this` will be referring to another execution context object in which `bar` is not defined. As written, this code will output "foobarbaz" and then throw a TypeError when trying to execute `this.bar()` since bar is not a function but is being invoked as such.
You can use the `bind` function so that the execution context object persists within the anonymous function call by adding `.bind(this)` to the first parameter of the setTimeout call. You can also store `this` in a variable within the `foo` function, ie `var self = this;`, and then call `self.bar()` instead of `this.bar()`.

**Question 4:**

```javascript
// Given an array of integer values, write a function in any language that returns an array
// of the duplicate values in that array. So in the array [1, 2, 3, 3, 4, 5, 5, 6] the returned
// array should be [3, 5].
```

function findDuplicates(array) {
  var seen = {},
      dups = [];

  array.forEach(function(el) {
    if (!seen[el]) {
      seen[el] = true;
    } else if (seen[el] === true) {
      seen[el] = 1;
      dups.push(el);
    }
  });

  return dups;
}

## Part 2

Consider the following models. Assume these models exist in a PostgreSQL database with the ORM ActiveRecord siting on top.

```javascript
/*
Company
  - id
  - name (string)
  - description (text)
  - logo_url (string)
  - score (int)
  - has many funding rounds

FundingRound
  - id (int)
  - funding_in_usd (float)
  - company_id
*/
```

**Question 5:**

When using an ORM what techniques could you use to ensure the fastest possible lookup time of the above models and the relationships between those models?

Create a ```:has_many funding_rounds``` association in Company model, with a ```:belongs_to``` association in the FundingRound Model.  Eager loading models using include method in controller instead of iterating over in view.

**Question 6:**

Design a RESTful JSON CRUD API for the 2 models above. Include expected input, what the controller might look like as well as the HTTP response code & JSON response object. Be sure to include how to obtain a list of each model type. Assume there are no security considerations expected in the solution. An example is provided below to give context as to what we are expecting in this question.

```javascript
// GET /funding_rounds/:id
//   Controller Process:
//     - Lookup Model, 404 on fail
//     - Render Model as JSON, 200 success
//   Input Example: {id: 123}
//   Output Example: {funding_round: { id: 123, funding_in_usd: 123.25, company_id: 321 }}
```
```
// GET /companies
//   Controller Process:
//    - Lookup Models, 404 on fail
//    - Render Models as JSON, 200 success
//   Input Example:
//   Output Example: { companies: [ { id: , name: , description: , ...}, {}... ] }

// GET /funding_rounds
//   Controller Process:
//    - Lookup Models, 404 on fail
//    - Render Models as JSON, 200 success
//   Input Example:
//   Output Example: { funding_rounds: [ { id: , funding_in_usd: ,  ...}, {}... ] }

// GET /companies/id
//   Controller Process:
//    - Lookup Model, 404 on fail
//    - Render Model as JSON, 200 success
//   Input Example: {id: 12}
//   Output Example: { company: { id:12 , name: "company12", description: , ...} }

// POST /companies
//   Controller Process:
//    - Save Model
//      - 422 Unprocessable Entity on fail, redirect to companies/new
//      - 303 redirect with address to new resource
//   Input Example: { name: "New Company", description: "LLama supplements research", logo_url: "http://..." }
//   Output Example: { company: { id: 32, name: "New Company", ...} } (after redirect)

// PUT /companies/:id
//   Controller Process:
//    - Update Model, 500 on fail
//    - 303 redirect with address to new resource
//   Input Example: { id: 9, name: "Fixed Company" }
//   Output Example: { company: {id: 9, name: "Fixed Company" } } (after redirect)

// DELETE /companies/:id
//   Controller Process:
//    - Delete Model, 404 on fail
//    - 200 on success
//   Input Example: { id: 9 }
//   Output Example: 


```



**Question 7:**


[Live Link](http://www.djc.space/private/companies.html)

Please See companies.html, with accompanying stylesheets in CSS folder.

Using HTML & CSS, take the following mock and implement a list of company models. The JSON output you have on the front-end should be the JSON output for the `/companies` API route you created in question 2. You are not expected to be tying data in javascript objects to HTML elements in this question. You are only expected to create an HTML file that implements the mocks.

Mock:

https://s3.amazonaws.com/ryse-static/programming-test/mock.png

Company Logo Examples:

https://s3.amazonaws.com/ryse-static/programming-test/facebook.png,
https://s3.amazonaws.com/ryse-static/programming-test/google.png,
https://s3.amazonaws.com/ryse-static/programming-test/reelio.png,
https://s3.amazonaws.com/ryse-static/programming-test/twitter.jpg

Typography:
```css
font-family: Open Sans, sans-serif;
color: #727272;
```

Background:
```css
background-color: #f1f1f1;
```
