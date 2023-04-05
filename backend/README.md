#  Product Recommendation Web Application - Backend

This is the backend for the Product Recommendation Web Application, a web application that allows users to browse and purchase products and provides product recommendations based on user preferences.

## Technologies
- `Express` for the backend
- `Neo4j` for the database

## configure the application
to run this backend you need to create a `.env` file in the backend folder containing thid
```ini
PORT=5000
NEO4J_URL=bold://your-url
NEO4J_USERNAME=your-username
NEO4J_PASSWORD=your-password
```
then you can go and run npm 
```bash
cd backend
npm install
npm start
```
## end-points


### users
- `POST /login:` This endpoint should accept a login request from a user and verify their credentials. If the login is successful, it should return an access token or other authentication token that can be used to authenticate future requests. If the login is unsuccessful, it shou ld return an error.<br/><br/>
- `POST /register:` This endpoint should accept a new user registration request and create a new user in the database. This might involve validating the request data and hashing the user's password for security.<br/><br/>
- `GET /users:` This endpoint should return a list of all users in the database. This might be useful for administrative purposes.<br/><br/>
- `GET /users/:id:` This endpoint should return a single user with the specified ID. This might be useful for displaying user profiles or for retrieving user information for other purposes.<br/><br/>
- `POST /users:` This endpoint should accept a new user registration request and create a new user in the database. This might involve validating the request data and hashing the user's password for security.<br/><br/>
- `PATCH /users/:id:` This endpoint should accept updates to a user's profile information and store the updates in the database. This might involve verifying that the user has permission to update their own profile.<br/><br/>
- `DELETE /users/:id:` This endpoint should delete a user from the database. This might be useful for handling account deletion requests or for administrative purposes.

### products

- `GET /products`: This endpoint should return a list of all products in the database.<br/><br/>
- `GET /products/:id`: This endpoint should return a single product with the specified ID.<br/><br/>
- `POST /products`: This endpoint should accept a new product submission request and create a new product in the database. This might involve validating the request data and storing any relevant images or other media.<br/><br/>
- `PATCH /products/:id`: This endpoint should accept updates to a product's information and store the updates in the database. This might involve verifying that the user has permission to update the product.<br/><br/>
- `DELETE /products/:id`: This endpoint should delete a product from the database. This might be useful for handling product removal requests or for administrative purposes.<br/><br/>
- `GET /products/category/:category`: This endpoint should return a list of products in the specified category.<br/><br/>
- `GET /products/search?query=:query`: This endpoint should return a list of products that match the specified search query.<br/><br/>


### carts

- `GET /carts`: This endpoint should return a list of all carts in the database.<br/><br/>
- `GET /carts/:id`: This endpoint should return a single cart with the specified ID.<br/><br/>
- `POST /carts`: This endpoint should accept a new cart creation request and create a new cart in the database. This might involve validating the request data and creating any necessary relationships between the cart and the products it contains.<br/><br/>
- `PATCH /carts/:id`: This endpoint should accept updates to a cart's information and store the updates in the database. This might involve verifying that the user has permission to update the cart and updating any necessary relationships between the cart and the products it contains.<br/><br/>
- `DELETE /carts/:id`: This endpoint should delete a cart from the database. This might be useful for handling cart removal requests or for administrative purposes.<br/><br/>
- `POST /carts/:id/products`: This endpoint should add a product to the specified cart. This might involve creating a new `INCLUDES` relationship between the cart and the product.<br/><br/>
- `DELETE /carts/:id/products/:productId`: This endpoint should remove a product from the specified cart. This might involve deleting the `INCLUDES` relationship between the cart and the product.<br/><br/>

### orders

- `GET /orders`: This endpoint should return a list of all orders in the database.<br/><br/>
- `GET /orders/:id`: This endpoint should return a single order with the specified ID.<br/><br/>
- `POST /orders`: This endpoint should accept a new order submission request and create a new order in the database. This might involve validating the request data, creating any necessary relationships between the order and the products it contains, and updating the inventory levels of the products.<br/><br/>
- `PATCH /orders/:id`: This endpoint should accept updates to an order's information and store the updates in the database. This might involve verifying that the user has permission to update the order and updating any necessary relationships between the order and the products it contains.<br/><br/>
- `DELETE /orders/:id`: This endpoint should delete an order from the database. This might be useful for handling order cancellation requests or for administrative purposes.<br/><br/>
- `GET /orders/user/:userId`: This endpoint should return a list of orders for the specified user.<br/><br/>
- `GET /orders/status/:status`: This endpoint should return a list of orders with the specified status.<br/><br>


# DATA MODELING
data model for an ecommerce website that includes a recommendation system based on product ratings and user purchase history:

## Nodes:

`User`: Each user will have a unique identifier (e.g. an email address or user ID), as well as properties such as name, address, and payment information.<br/>
`Product`: Each product will have a unique identifier (e.g. a product ID), as well as properties such as name, price, and category.<br/>
`Order`: Each order will have a unique identifier (e.g. an order number), as well as properties such as the date of the order, the total cost, and the status (e.g. "completed", "cancelled", etc.).
<br/>

## Relationships:

`BOUGHT`: This relationship will connect a User node to a Product node, and will represent that the user has purchased the product. You might want to include properties on this relationship such as the date of the purchase and the rating that the user gave to the product.<br/>
`IN_CART`: This relationship will connect a User node to a Product node, and will represent that the product is currently in the user's shopping cart.<br/>
`PART_OF`: This relationship will connect an Order node to a Product node, and will represent that the product was included in the order.
<br/><br/><br/>

# working with Cypher (neo4j)
- ### `To create a node:`
```sql
CREATE (n:Label {properties})
RETURN n
```

- ### `Example: with known id`
```sql
CREATE (n:Product {id: 1, name: "Product 1", price: 99.99})
RETURN n
```

- ### `Example: with generated id`
```sql
CREATE (n:Product {id: apoc.create.uuid(), name: "Product 1", price: 99.99})
RETURN n
```

- ### `To delete a node:`
```sql
MATCH (n:Label)
WHERE n.property = value
DELETE n
```

- ### `Example:`
```sql
MATCH (n:Product)
WHERE n.name = "ROG ROG ROG"
DELETE n
```
- ### `To update a node:`

```sql
MATCH (n:Label)
WHERE n.property = value
SET n.property = newValue
RETURN n
```
- ### `Example:`
```sql
MATCH (n:Product)
WHERE n.name = "ROG ROG ROG"
SET n.price = 119.99
RETURN n
```
- ### `To create a relationship between two nodes:`
```sql
MATCH (n1:Label1), (n2:Label2)
WHERE n1.property = value1 AND n2.property = value2
CREATE (n1)-[r:Type]->(n2)
RETURN r
```

- ### `Example:`
```sql
MATCH (p1:Product), (p2:Product)
WHERE p1.name = "Mac Book Pro 2022" AND p2.name = "ROG ROG ROG"
CREATE (p1)-[r:RELATED_TO]->(p2)
RETURN r
```

- ### `Access to a node through relation`
```sql
MATCH (p1:Product)-[r:RELATED_TO]->(p2:Product)
WHERE p1.name = "ROG ROG ROG"
RETURN p2
```