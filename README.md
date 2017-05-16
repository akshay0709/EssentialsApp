# Gourmet.com

Hungry?? Hang in there!!! You have got Gourmet.com at your service. A web application specifically build to cater the hunger needs. Gourmet.com gives you the privilege to choose the cuisine that your taste buds might be craving for. It provides a diverse palette of dine-in options in your close proximity. You can also go ahead and search for any specific restaurant. You choose your restaurant upon looking at the ratings and cost estimates. This application allows you to opine on the quality of your search and satisfaction in real time.

## Setup And Installation:

This application is currently running on local machines. In order to use the application and its’ features, the user must download the following requirements and follow the listed instructions.
 
Windows Users:
- Clone the project from the following github url: (https://github.com/hchaundkar/EssentialsApp)
- Download and install node.js from https://nodejs.org/  
- “npm install” for installing all the node modules. 
- “bower install” for managing front end components. 
- Install Ember.js and all its dependencies.
- Generate Firebase account.
- Install Firebase plugins and dependencies.
- Create access token for Yelp-fusion api from url:(https://www.yelp.com/developers/documentation/v3/authentication)
- Run the website “Gourmet.com” using the following url:  (http://localhost:4200/)

## Technologies used:

1.       Ember.js (Client Side MVC)
2.       Node.js
3.       FireBase (Real Time Database)
4.       Bootstrap, CSS (Styling).

## Running the Application
Run the project by using following commands -
```
cd essentialsserver
npm run dev
```
Open another terminal for running the backend and use following command - 
```
cd esentialsclient
ember server
```

Note: For the connection with the yelp api we need a secret client id and key which need to be generated as stated in Setup and installation section. We need to update the value of below fields in index.js

```
const clientId = "clientid";
const clientSecret = "clientkey";
```


## Funtionalities:

- Search bar for finding a particular restaurant.
- Find all nearby restaurant by specific cuisine.
- Allows you to discover particular restaurant nearby.
- Specify location to find restaurants in that area.
- Give reviews about our website.
- Proves as a useful website giving reviews and restaurant in nearby places.
- Realtime reviews display.
- Firebase plugin renders all the comments in real time.


## Technical Details:
#### Server side code:
##### Node.js Server

* The server is used to call the Yelp API and return the data to the client in form of JSON.
* The server was used to have more control over the data retrieved from Yelp. Only the data necessary for the gourmet.com was returned from server side and other data returned from Yelp was eliminated.
* The Node.js server was designed to detect the keyword "search" in the request URL. If detected, the two parameter values "term" and "location" was passed to the Yelp API to get the relevant data. (Eg: term = food and location= Fullerton would return all the food joints in Fullerton).
```javascript
var server = http.createServer(function(req, res) {
      var parameters = url.parse(req.url, true);
      var query = parameters.query;
      var reqTerm = query["term"];
      var reqLoc = query["location"];
      console.log(parameters);
      if(parameters.pathname.includes('search')){
        console.log("In Search if");
        yelp.accessToken(clientId, clientSecret).then(response => {
           const token = response.jsonBody.access_token;
           const client = yelp.client(token);
           client.search({
               term: reqTerm,
               location: reqLoc
           }).then(response => {
             
                res.setHeader('Content-Type', 'application.json');
                res.end(JSON.stringify(response.jsonBody.businesses));
           });
       });
    }
```

#### Client side code:
##### Ember app
* In searchpage there are two input fields which accepts "term" and "location". (Eg: term=food and location="Fullerton" will display all the food joints in Fullerton)
* After submitting the values, alias of the model was created in the controller and returned the values to the "input" action method of the route.
```javascript
export default Ember.Controller.extend({
  search: Ember.computed.alias('model.search'),
  actions: {
    displayres() {
      var self = this;
      var re = this.get('search');
      var food = re.term;
      var location = re.term;
      self.send('input', re);
    }
  }
});
```
* In route, Object was created for the model and in "input" action method the values were passed to the "search" route as parameters.
```javascript
model(){
    return Ember.RSVP.hash({
      search: Ember.Object.create()
    });
},
actions: {
  input: function(response){
    this.transitionTo('search', response.term, response.location);
  }
}
```
* In search route, the accepted parameters were passed as parameters to the Node.js server and data was returned into the Ember model.
```javascript
export default Ember.Route.extend({
  model(params){
    var host = this.store.adapterFor('searchapplication').get('host');
    var url = host + '/search?term='+params.term+'&location='+params.location+'';
    return Ember.$.getJSON(url);
  }
});
```
* The returned data was then displayed in the search results.



## User Manual:

Step 1:

- Launch the application using url (http://localhost:4200)
- The web page will look like the screenshot below.

![homepage](https://cloud.githubusercontent.com/assets/25421655/26038737/17c6119e-38c4-11e7-969a-18091e340f9a.jpg)

- The HomePage has the below functionalities:
- Home : This renders you to the Homepage
- Search: This renders you to the SearchPage
- Reviews: This renders you to add reviews to our website.
- About : Thi displays the information about the team members.


Step 2:

- When you click on search link below page will be displayed.
![searchpage](https://cloud.githubusercontent.com/assets/25421655/26038740/17c7e398-38c4-11e7-96ae-9bb052865012.jpg)
- It has two options:
- Search by Cuisine
- Search by location
- When you hit on submit after entering the cusine and location it will display the below page with all the restauratnts available in that location.
![searchresult](https://cloud.githubusercontent.com/assets/25421655/26038738/17c658c0-38c4-11e7-8ade-a3c3666b7d64.jpg)



Step 3:

- When you click on RateUs links below page will be displayed.
- The page displays the existing reviews.
![reviews](https://cloud.githubusercontent.com/assets/25421655/26038739/17c7e78a-38c4-11e7-944a-3eca47d8ea29.jpg)
-  When you click on RateUs button the below page will be displayed.
- You can add a review about our webpage in the fields provided.

![rateus](https://cloud.githubusercontent.com/assets/25421655/26038736/17c5e84a-38c4-11e7-90a5-c6cf09c569f6.jpg)
Step 4:

- The About pages displays all the information about our development team.
![about](https://cloud.githubusercontent.com/assets/25421655/26038770/a7223296-38c4-11e7-988c-cb1db7af5984.jpg)





#### Team Members:

- Akshay Pawar 
- Harshada Chaundkar
- Manali Tawde
- Remilda Dsilva
- Tushar Kamble
