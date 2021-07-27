# How to create a pet store page using Swagger Petstore API

Have you ever wanted to create a page for your petstore ? Well, look no further! Swagger Petstore API is a Petstore server that gives you simple, easy to use functions that can allow you to various pet, store and user related actions. Some of which are as follows,
- Upload a picture of a pet
- Add a pet to the store
- Update an existing pet
- Get a list of pets
- Get a pet
- Delete a pet

The purpose of this article is to fetch a list of pets from the api and render the results in a web page.

## Fetch a list of pets
#### Request Url: https://petstore.swagger.io/v2/pet/findByStatus
#### Query Param: status 
Available values for status are 'available', 'pending' and 'store'. 

To see a list of pets that are available, paste the below link in the browser 
**https://petstore.swagger.io/v2/pet/findByStatus?status=available**

You should see an xml structure showing a list of pets. XML structures follow a tree structure with one parent node and several children or leaf nodes.
<pets> is the parent node indicating an array or a list of pets. <Pet> is a single leaf node describing a single pet. A pet object from the server api has 
- a category with an id and a name,  
- a pet id
- name
- image url
- status 
- tag, an identifier that describes the pet with keywords and also has an id and a name

        <pets> 
            <Pet>
                <category>
                    <id>0</id>
                    <name>string</name>
                </category>
                <id>9222968140497180179</id>
                <name>test</name>
                <photoUrls>
                <photoUrl>string</photoUrl>
                </photoUrls>
                <status>available</status>
                <tags>
                    <tag>
                        <id>0</id>
                        <name>string</name>
                    </tag>
                </tags>
            </Pet>
        </pets>

## Display the list of pets in a html page
Let's start with a simple index.html page with the below code.

        <!DOCTYPE html>
        <html lang="en">
            <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-      scale=1.0">
                <title>My Pet Store</title>
                <script src='index.js' defer></script>
            </head>
            <body>
                <div id='pets'></div>
            </body>
        </html>

In index.js we will add the below code used to fetch a list of pets from the swagger petstore api, which returns a response object. response.json() returns a json version of the response instead of xml. 'pets' is the response in json array format. Finally showPets function is called to display an image and name of the pet.

        fetch(‘https://petstore.swagger.io/v2/pet/findByStatus?status=available’)
           .then(response => response.json())
           .then(pets => showPets(pets));

        showPets = pets => {
          const petsDiv = document.querySelector(‘#pets’);
          pets.forEach(pet => {
            const imgElement = document.createElement(‘img’);
            const nameElement = document.createElement(‘p’);
            const containerDiv = document.createElement(‘div’);
            if (pet.photoUrls && pet.photoUrls.length>0) {
                imgElement.src = pet.photoUrls[0];
            }
            nameElement.innerText = `Pet Name: ${pet.name}`;
            containerDiv.append(imgElement);
            containerDiv.append(nameElement);
            petsDiv.append(containerDiv);
          });
        }

We will now navigate to the location of the index.html page and open it in a browser, and we should see a list of images and name of the pets.
