# PBI

How is mapping all fields between the models that is send to the UI and entities that are modeled in the database.  
#### Users 

|Model          | Entity                                        | Type      |  
|--------       |---------                                      |-------    |
|Id             |Id                                             |[guid]     |
|UserName       |UserName                                       |[string]   |
|Name           |Contact.Name (first + middle + last name)      |[string]   |
|Email          |Contact.ContactEmail.FirstOrDefault().Email    |[string]   |
|Phone          |Contact.ContactPhone.FirstOrDefault().Phone    |[string]   |
|Roles          |Join (Roles.Select(x => x.Name))               |[string]   |
|Enable         |Enable                                         |[boolean]  |
|UserType       |Contact.EntityMetadata.Name                    |[string]   |
|CreateDate     |Contact.CreateDate                             |[DateTime] |
|ContactId      |ContactId                                      |[Guid]     |

- Note[***Done***]: Aquí faltaba poner el include para que trajera el email y Phone y el Name de la base de datos , no lo estaba trayendo y por eso aparecian vacios esos campos

2- Added a new field **EntityType** of type ***guid*** in the filter of endpoint ```GET /UserManager``` , to filter by ***EntityMetadataId*** the type that the user's contact has user.  

3- There is a Endpoint for get all EntityMetada from Database. The Endpoint is ```GET /EntityMetadata``` 

4- In endpoint ```GET -> /Contacts``` in the model que se le entrega al UI estan los campos Email, Name y el EntityMetadataId.

5- Updating and changing endpoint of Identity Server Project 

- Changed the route to a new route for Identity Server, before the route was ```{server:port}/identity/api/auth/{endpoint}```, now has the format: ```{server:port}/identity{endpoint}```, the substring ```api/auth``` was removed from path. 
- Updated The Endpoint ```POST /register```, now the model that come in the body have the belove structure. 

    ```
    user:  [string]      -- unique and valid user name. 
    email: [string]      -- should have a valid structure of a email 
    pass:  [string]      -- should have required validations for passwords 
    phone: [string]      -- should have a valid structure of a phone number  
    ```
- Added a new validation for avoid repeted records in fields user , email and phone  
- Changed the format for ```POST /forgotPassword```, before that endpoint had a body with a field **email** (the user's email that forgot the password), now the **email** comes in the route, ```POST /forgot/{email}```. the absolute route now is ```{server:port}/identity/forgot/{email}``` 
- Change the format for ``` POST /resetPassword```, before that endpoint had that  format , now have the format: ```POST /reset``` with the same body: 
    ```
    email:      [string]     -- should be a email of a user.             
    password:   [string]     -- the new password, with required validations for password.
    code:       [string]     -- the code for validate the new password. 
    ```



