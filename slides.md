---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Node.js - part 5/12
Back-End Development
- [ ] Handle HTTP requests (GET, POST, PUT, DELETE)
- [ ] Create routing in Express
- [ ] Handle query strings and parameters 

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
- list some HTTP methods
- show websites that use this
-->

---
transition: slide-left
---

# Exercise1 : Routing
(30 min) Refer to [last class slides](https://unit03-lesson04.netlify.app/presenter/2)

```js
1. create a new folder and go into it via `cd folder`
2. Type `npm init -y`
3. `npm install express` // where in package.json is express placed?
4. `npm install nodemon --save-dev` // what does --save-dev do? where in package.json is nodemon placed?
5. create an index.js file with the following code:
```
```js
const express = require('express');
const app = express();

// Mock data for food truck orders
let orders = [
  { id: 1, item: 'Taco', quantity: 2, customer: 'Alice' },
  { id: 2, item: 'Burger', quantity: 1, customer: 'Bob' },
];

app.get('/orders', (req, res) => { res.json(orders) });
app.listen(3000, () => { console.log('Server running on http://localhost:3000') });
```
```js
6. make it so you run the app via `npm start` and it uses nodemon
7. Move the port number in .env 
```

Goto next slide...

<!--
-->

---
transition: slide-left
---

# Exercise 1 continued: Routing

```js
// 8. Create a new route that gives us a single order by ID
app.get('/orders/:id', (req, res) => { // What does the : followed by any name do?
  // 9. Fill in the ??? below so that we get the value of :id. Hint: console.log(req.params)
  const orderId = parseInt(???); // What does parseInt do?

  // 10. Fill in the ??? below such that "order" will have the object where its id equals orderId
  const order = orders.find(???); // What does the array method .find do? See 

  // 11. Fill in the ??? below that gives us a truthy value if the above order was found
  if (???) {
    res.json(order);
  } else {
    res.status(404).json({ error: 'Order not found' });
  }
});
```
Goto next slide...

---
transition: slide-left
---

# Exercise 1 continued

Discuss:
- What steps does the server take from the moment a GET /orders/:id request is received to when it sends a response?  Walk through the flow, including route matching, parsing the ID, and sending the result?
- What would the response look like if an order is found vs. not found?  Describe both the JSON structure and the HTTP status codes involved?
- What data type is req.params.id, and why do we use parseInt() on it?  What would happen if we skipped parsing it, and how would that affect the lookup?

Save and push your code to GitHub; ignore any folders/files that don't need to be uploaded

---
transition: slide-left
---

# Exercise 2: Query Strings and Parameters
(50 min) Extract query strings and parameters from a url

- Add a new route: GET /orders/search?customer=Alice
- This should return all orders made by the customer named Alice.

```js
// 1. Adjust our array to include one more order from Alice
let orders = [
  { id: 1, item: 'Taco', quantity: 2, customer: 'Alice' },
  { id: 2, item: 'Burger', quantity: 1, customer: 'Bob' },
  { id: 3, item: 'Nachos', quantity: 1, customer: 'Alice' },
];
```
Goto next slide...

<!--
-->

---
transition: slide-left
---

# Exercise 2 continued: Query Strings and Params

```js
// 2. Fill in the ??? below to properly add the route
app.???('/orders/search', (req, res) => {
  // 3. Fill in the ??? so that if GET request is /orders/search?customer=Alice, then the variable customer will contain the value of "Alice". Hint: try console.log(req.query)
  const { ??? } = ???; // Review: here we are object destructuring to get customer
  let filteredOrders = [];

  // 4. Fill in the ??? such that the conditional is truthy if a customer is found
  if (???) {
    // 5. Fill in the ??? such that we only get Alice's/alice's orders
    filteredOrders = ???
  }

  res.json(filteredOrders);
});
// 6. Refactor code to also include query param item such that
//  if there is a GET /orders/search?customer=alice&item=taco
//  or if there is a GET /orders/search?item=taco&customer=alice
//  then either way it'll return { id: 1, item: 'Taco', quantity: 2, customer: 'Alice' }
```

Goto next slide...


<!--
-->

---
transition: slide-left
---

# Exercise 2 continued

Discuss:
- What is the difference between route parameters (/orders/:id) and query parameters (/orders/search?customer=Alice)?  When and why would you use one over the other?
- How might you modify the /orders/search route to allow partial matches (e.g., search for “Ali” to get “Alice”)?
- How might you handle pagination in the /orders/search route? ex: ?page=2&limit=10, and how would that change the response structure?

Remember to save to GitHub

---
layout: image-right
transition: slide-left
image: /assets/dodds.png
backgroundSize: 500px 140px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 📤 [Request object DOCS](https://expressjs.com/en/api.html#req)
- 📥 [Response object DOCS](https://expressjs.com/en/api.html#res)
- 📁 [Send a file DOCS](https://expressjs.com/en/api.html#res.sendFile)
- 🍵 [Express web framework MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

<!-- 
- take attendance
-->

---
transition: slide-left
---

# Exercise 3: POST
(50 min) Review https://unit03-lesson02.netlify.app/12

- Add a new route: POST /orders; this should add a new order 
- Request's body should contain the new data for the order (item, quantity, customer)

```js
// 1. Fill in the ??? to add the POST route
app.???('/orders', (req, res) => {
  const { item, quantity, customer } = req.body;

  const newOrder = {
    id: ???, // 2. Fill in the ??? such that we set the next sequential id number
    item,
    quantity,
    customer,
  };
  
  // 3. Fill in the ??? such that orders should have a list of all the orders including the newest one
  orders???

  // 4. Fill ??? to be a status code signifying the request succeeded and a new resource was created
  res.status(???).json(newOrder);
});
```

Goto next slide...

<!--
-->

---
transition: slide-left
---

# Exercise 3 continued

Discuss:
- What HTTP method is used to create a new order, and why is it appropriate here?  Why not use GET instead?
- Where does the server get the new order data from in the request?
- How could you validate the incoming request data to make sure it's complete and valid?
What would happen if a user sends a request without item, quantity, or customer?

Remember to save to GitHub

---
transition: slide-left
---

# Exercise 4: PUT 

- Add a new route: PUT /orders; this should update an order by id 
- Request's body should contain the new data for the order (item, quantity, customer)

```js
app.put('/orders/:id', (req, res) => {
  const orderId = parseInt(req.params.id);
  const { item, quantity, customer } = req.body;
  const index = orders.findIndex((o) => o.id === orderId);

  if (index !== -1) { // My font ligature makes the "not equals" operator look like that in the slides
    orders[index] = { id: orderId, item, quantity, customer };
    res.json(orders[index]);
  } else {
    res.status(404).json({ error: 'Order not found' });
  }
});
```

- Test with POSTMAN to ensure it works
- Go to next slide...

---
transition: slide-left
---

# Exercise 4 continued

Discuss:
- What is the purpose of the PUT method in this route and how is it different from a POST?
- Why did we use findIndex() instead of find() when updating?  (i.e. What do we need that find() doesn’t give us?)
- How can you validate the data before updating? What if someone tries to update an order with quantity: "hello"?

Remember to save to GitHub

---
transition: slide-left
---

# Exercise 4 continued: DELETE

- Add a new route: Delete /orders/:id should delete an order by id 

```js
app.delete('/orders/:id', (req, res) => {
  const orderId = parseInt(req.params.id);
  const index = orders.findIndex((o) => o.id === orderId);

  if (index !== -1) {
    orders.splice(index, 1);
    res.sendStatus(204); // What does status 204 mean?
  } else {
    res.status(404).json({ error: 'Order not found' });
  }
});
```
- Test with POSTMAN to ensure it works

---
transition: slide-left
---

# Exercise 4 continued

Discuss:
- How does the server know which order to delete?
- Should anyone be allowed to delete an order?
- What are "soft deletes", why would you use it and how would you implement it?

Remember to save to GitHub

---
transition: slide-left
---

# Homework

- Attempt https://www.theodinproject.com/paths/full-stack-javascript/courses/nodejs
- Download MongoDB's [compass](https://www.mongodb.com/products/tools/compass) and also sign up to create an account
