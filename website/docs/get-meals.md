---
sidebar_position: 4
---

# Get meals

<table>
	<tr>
		<td>URL: </td>
		<td>[Data Provider Base URL]/openmeal/v2/meals.json</td>
	</tr>
	<tr>
		<td>HTTP Method: </td>
		<td>GET</td>
	</tr>
	<tr>
		<td>Parameters: </td>
		<td><ul>
			<li>distributorID - parameter specifying the Data Provider ID for a Distributor, used to return only the meals for that Distributor. If not provided meals for all Distributors are returned in the cases when the Data Provider has implemented this feature, otherwise a <i>501 Not Implemented</i> error is returned.</li>
			<li>startDate - optional parameter on the format YYYY-MM-DD specifying the start of the period to get meals for. If not specified todays data will be used as default.</li>
			<li>endDate - optional parameter on the format YYYY-MM-DD specifying the end of the period to get meals for. If not specified the next 7 days will be used as default.</li>
		</ul></td>
	</tr>
	<tr>
		<td>Formats: </td>
		<td>JSON</td>
	</tr>
</table>

## Response

All available meals corresponding to the methods parameter are returned. The basic structure of the response is as follows...

| Property     | Type                         | Description                                                                                             | Required? |
| ------------ | ---------------------------- | ------------------------------------------------------------------------------------------------------- | --------- |
| distributors | Array of Distributor Objects | A list of all the distributors for the meal (if the same meal is served in several schools for example. | Yes       |
| meals        | Array of Meal objects        | The meals served within the set dates                                                                   | Yes       |

###Distributor
Same as the Distributor object returned by the [List Distributors](/doc/list-distributors.html) method.

###Meal
The Meal object describes a whole meal, such as a lunch or a dinner, including all alternative dishes.

| Property    | Type                    | Description                                                                                                                                                                    | Required? |
| ----------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| date        | String                  | The data of the meal in [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) format, for example 2015-05-20T00:00:00                                                              | Yes       |
| lang        | String                  | The 2-letter [ISO 639 alpha-2](http://en.wikipedia.org/wiki/ISO_639-1) language code describing the language used in all Strings of this object. For example _sv_ for Swedish. | Yes       |
| name        | String                  | The name of the meal, for example "Lunch"                                                                                                                                      | Yes       |
| description | String                  | An optional description of the meal                                                                                                                                            | No        |
| courses     | Array of Course Objects | All the alternative dishes for the meal                                                                                                                                        | Yes       |

###Course
The Course object describes a specific dish that is part of the Meal.

| Property           | Type             | Description                                                                                                                                                                                                                          | Required?                       |
| ------------------ | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------- |
| name               | String           | The name of the course, for example "Swedish Meatballs"                                                                                                                                                                              | Yes                             |
| description        | String           | An optional longer description of the course                                                                                                                                                                                         | No                              |
| order              | Integer          | Used to sort the courses in a prefered order, with lowest order value first                                                                                                                                                          | No                              |
| ingredients        | String           | Describes what ingredients, including allergens, the course contains according to [EU regulation 1169/2011](http://eur-lex.europa.eu/LexUriServ/LexUriServ.do?uri=OJ:L:2011:304:0018:0063:EN:PDF). Not meant to be used as a recipe. | No                              |
| allergens          | Array of Strings | Describes the allergens of the course by name.                                                                                                                                                                                       | No                              |
| nutrients          | Array of Objects | Describing the ingredients of the course one by one. If a nutrients array exists the information listed in it must correspond to what is in the _text_ property, so tht only the _text_ property can be used.                        | No                              |
| nutrients > name   | String           | The name of the nutrient                                                                                                                                                                                                             | Yes, if nutrients are specified |
| nutrients > amount | String           | The amount of the nutrient in a standard serving of the course                                                                                                                                                                       | Yes, if nutrients are specified |
| nutrients > unit   | String           | The unit the _amount_ is measured in                                                                                                                                                                                                 | Yes, if nutrients are specified |

## Example

    GET http://openmeal.foodindustries.inc/openmeal/v2/meals.json?startDate=2015-10-19

    {
        "data" : [
    	    {
        	    "distributors" : [
    			    {
    				    "name" : "The Local School",
    				    "lang" : "en",
    				    "address" : {
        				    "streetAddress" : "Main Street 42",
        				    "postalCode" : "555 55",
        				    "addressLocality" : "Stockholm",
    					    "addressCountry" : "SE"
    				    },
    				    "url" : "http://thelocalschool.se",
    				    "telephone" : "+46 555 555 555",
    				    "distributorID" : "123456789"
    			    },
    			    ...
    		    ],
    		    "meals" : [
    			    {
    		    	    "date" : "2015-05-20T00:00:00",
    		    	    "lang" : "en",
    		    	    "name" : "Lunch",
    		    	    "description" : "Swedish Food Week",
    				    "courses" : [
    		    		    "name" : "Swedish Meatballs with mashed potatoes",
    		    		    "order" : 1,
    					    "ingredients" : "Meat, bread crumbs, eggs, milk, potatoes",
    					    "allergens" : ["Egg", "Milk", "Nuts"],
    						"nutrients" : [
    							{
    								"name" : "Energy",
    								"amount" : "540",
    								"unit" : "kcal"
    							},
    							...
    						]
    				    ]
    			    },
    			    ...
    		    ]
    	    },
    	    ...
        ]
    }
