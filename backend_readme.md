## Backend (Node.js + Express)

1. mkdir backend
2. cd backend

3. npm init -y  --> [To Initialize a new node.js project]

   * This will install `package.json` file

4. npm install body-parser cores cors dotenv express mongoose --save     --> This will install 6 npm packages (Node.js modules) into your project:

    [ This will create `package-lock.json` and `node_modules` file]

    [ Also This command will update dependencies in `package.json` file. In `package.json` file in dependency field you can see mongoose, express, dotenv dependency along with version.]

    * express --> A web framework for building RESTful APIs or web applications in Node.js. Simplifies server-side logic and routing.

    * mongoose --> An Object Data Modeling (ODM) library for MongoDB and Node.js. Simplifies interaction with MongoDB by providing schema and query-building functionalities.

    what is express? purpose?

    * It is a lightweigt framework, that simplifies the process of creating web application and APIs.
    * Express simplifies request routing, making it easy to define endpoints for different HTTP method (GET, POST, PUT, DELETE etc.)


5. Now we paste the backend code from Geeks for Geeks.

    1. backend/controller/restaurent.controller.js:
    
        * Its job is to receive a request from the user, process the filters they provide (like location, rating, or cuisines), and then find matching restaurants in a database.

        Example:

            Imagine a restaurant app like Zomato or Swiggy. When you apply filters like:

            Location: Chennai
            Rating: 4 stars or above
            Cuisine: Indian and Chinese

            This function will search the database for restaurants that match your filters and send the results back to you.


    2. backend/models/restaurant.model.js

        * It's a Mongoose schema for a "restaurant". A schema acts like a blueprint, telling the database what a "restaurant" record should look like and what kind of data it should store.

        What’s a Schema?

            1. Think of it like a form you fill out when adding a restaurant to the system. It specifies what information a restaurant should have (e.g., name, address, contact info, etc.).

            2. The line module.exports = mongoose.model("restaurant", restaurantSchema); creates a model named "restaurant". A model is like a toolkit that lets you perform database operations (like adding, searching, or updating restaurants).

        Example:

            If you're building an app like Zomato or Swiggy:

                * This schema is what defines how restaurant data is stored in the database.
                * It ensures consistency so that every restaurant has the same type of information, making it easier to manage and query.

    
    3. backend/routes/restaurant.route.js


        Purpose: This route is like the "search bar" for your restaurant app. When someone searches for restaurants by specifying criteria like "location" or "rating," this route processes the search.

        Example in Real Life:

            * A user opens the app and searches for "restaurants in Chennai with a rating of 4 or more."
            * The app sends a POST request to /restaurants with the search filters.
           
                {
                    "location": "Chennai",
                    "rating": 4
                }

            * This route picks up the request and calls the getRestaurantsByFilters function to look for restaurants in the database that match the filters.
            * The filtered results (or a "No results found" message) are sent back to the user.

    4. backend/index.js

        * This code sets up a basic backend for a restaurant application.

        * It connects to a MongoDB database, fills it with sample restaurant data, and provides two routes:
            
            `/health` to check if the server is running.
            `/api/restaurants` to get the list of restaurants.


6. node index.js    -->  To start the server run the following command.

   Note:

    * Replace the mongo db url in index.js


7. backend is listening to port http://localhost:4000/api/restaurants 

8. Note:

        while building the docker Image --> please ensure that in index.js update the mongodb URL 

        * Currently we are using our mongodb in local machine. so i update the mongourl as "mongodb://host.docker.internal:27017/Restaurant_db" in index.js file.
        * If we use mongo as docker container then we need to change the mongourl based on that.

9. Instead of hardcoding the port and Ip address in backend we are getting those info from env variable from kubernetes cluster.

    * In backend `index.js` file we modify this file

            ```
            const PORT = process.env.PORT || 4000;
            const HOST = process.env.HOST || '127.0.0.1'; // Fallback to localhost for local development

            app.listen(PORT, HOST, () => {
            console.log(`Server running on http://${HOST}:${PORT}`);
            });
            ```
    
    * Also in `mongo url` we modify the hardcoded lines

            ```
             //   process.env.MONGO_URL || local mongo db;              

            const MONGO_URL = process.env.MONGO_URL || "mongodb://127.0.0.1:27017/Restaurant_db";   

            [or]

            //   process.env.MONGO_URL || docker mongo db container; 

            const MONGO_URL = process.env.MONGO_URL || "mongodb://host.docker.internal:27017/Restaurant_db"   

            ```




