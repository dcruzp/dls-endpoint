# PBI PTO

### Endpoints 

- ### ``` GET /PTO/Holidays``` 

    This endpoint return a list of all holidays. You can filter that results applying a filter to query. 

    - Query Specifiaction 
        ```
        fromdate      [datetime]      
        todate        [datetime]
        name          [string]
        pagenumber    [integer]
        pagesize      [integer]
        ```
        [NOTES] 
        ```fromdate``` is the date to compare all the dates of the ```Holidays.date``` filed where all are greather or equal than the ```fromdate``` field that come form filter.

        ```todate``` is the date to compare all the dates of the ```Holidays.date```  field where all are less or aqual than the ```todate``` field that come from filter. 

        ```name``` is the name of the Holiday that could by comparer with ```Holiday.name``` , the comparer could be a a **contain** funtion, in the way that if Holiday.name.Contain(name) then the filter function should return ***true***

        ```pagenumber``` is the page number the should be returned  when paginated the resultd . this field is a integer and should be greather than zero. 

        ```pagesize``` is the amount of records that should be returned in the paginated result. this should be a integer greather than zero    


    - Body Response    

        The body response is paginated, and the data that comes inside the paginates response, in the field data (that is a array) have the next structure:

        ```
        date:         [datetime] 
        name          [name] 
        ```

        [NOTE] 
        the ```date``` field is a ```datetime``` that is the date when is the holiday celebrate, and the ```name``` field is a string that represent the name by witch is known.  

       
- ```POST /PTO/Holidays```
    This endpoint insert a new Holiday in the table ```Holidays```. The body of this endpoint have the next structure: 

    - Body Structure
        ```
        date          [datetime]
        name          [string]
        ```

        [Note] 
            ```date``` field should have the structure of a datetime type, and ``name`` field is a string that shouldn't be more longer than 50 charactres. 

- ```GET /PTO/```
    This endpoint return a list of PTO that whas made for agents in the application. The result cna by filtered by a filter that will be descibed below. and the results will comes in a paginated structure. 

    - Query Specifiaction 
        ```
        fromdate      [datetime]      
        todate        [datetime]
        agent         [guid]
        pagenumber    [integer]
        pagesize      [integer]
        ```
        [NOTES] 
        ```fromdate``` is the field that tell you from what date you want filtr the PTO, the filed in the database for comparison is ``PTO.CreatedDate``

        ```todate``` is the field that tell you to what date you want filtr the PTO, the filed in the database for comparison is ``PTO.CreatedDate``

        ```agent``` is a ``Guid`` field that have to be comparer with the field in database ``PTO.agentId``.   

        ```pagenumber``` is the page number the should be returned  when paginated the resultd . this field is a integer and should be greather than zero. 

        ```pagesize``` is the amount of records that should be returned in the paginated result. this should be a integer greather than zero.

    - Body Response 

        The response body is a paginated respose that have  the next structure in the `data` field.  

        ```
        id          [guid]         - the id of the PTO record in database
        agentId     [guid]         - id of agent that request the PTO
        statusId    [guid]         - the id of the status for that PTO
        status      [string]       - The name of the status 
        comment     [string]       - this is a comment for the PTO request
        details     [list]  this is a  array of detils 
        ```
        [NOTE] details field is a array of records that describe the days when the PTO is requested. The structure if this elements will be describe below.  
        
        - Datail Structure
            ```
            id:         [guid] 
            startdate   [datetime] 
            enddate     [datetime]
            start:      [integer] 
            hours:      [integer]
            ```
            [NOTE]
            - `id` field is teh is that that record have in the database. 
            - `startdate` field is the date where the PTO details begin.
            - `enddate` field is the date until what date is the solicitude
            - `start` is the hour when the solisitude begin
            - `hours` is the amount of hours of the solicitude 

- ```POST /PTO/newpto``` 
    This endpoint have the funtionality to create a new request of PTO for a Agent given. The body request have the below specifications. 

    - Body request 
        ```
        agentId:       [guid] 
        ptoDetails:    [array] - the details descritons 
        ```
        [NOTE] 
        The `ptoDetails` field is a array where are described the days that the agent are requested the vacations. The Details array elemetns have the next structure. 

        - `ptoDetails` Structure 
            ```
            date:           [datetime]    - the days of vacation 
            start:          [integer]     - at hour where day begin
            hours:          [datetime]    - the amount of hours to work at day
            ```
        
- ```POST /PTO/update```

    This endpoint is for update the status and datails of a vacation request. The structure for that is decrubed below: 

    - Body Request
    ```
        ptoId:              [guid]
        agentId:            [guid]
        statusId:           [guid]
        ptoDetails:         [array]   -descrition of PTO details 
    ```

    >[**Note:]** 
        >- `ptoId`  field is the Key in the database of that request. That is required for find and can be updated 
        >- `agentid` field is the id of the agent that request the vacations. 
        >- `statusId` is the status current status for that request of vacations. This status can have the next values described belove: 
        >    ```
        >    Denied          ->   ("C62FC6DC-47C0-4BA2-B765-0210957EBEA6")
        >    Approved        ->   ("03C76016-1A1B-4396-AF10-60FC047FA958")
        >    Requested       ->   ("461A9258-18BE-40DF-8810-C5D6F829B0DC")
        >    Cancelled       ->   ("F894A68F-6041-41FD-9804-F3555B27275B")
        >    ``` 
        >- `ptodetails` structure have the same structure that `ptodetails` field in >endpoint  `POST PTO/newPTO`




### Type Generic and Metadata added at database for this functionality 

 This records are news in TypeGeneric table in Database, and are use for represent the types of Holidays. 

|TypeGenericId                        | EntityMetadataId                    | Code     |   Name          |  
| ------------------------------------| ------------------------------------|----------|-----------------|
|9AA5A59A-3969-4ADC-9798-03B656F21E9A |2E2170B2-10F1-409E-8FA9-7461D7E39BDD |HDW       |Holiday WeekDay  |
|3034A348-ACFC-4E0A-AC8A-2DA03C451320 |2E2170B2-10F1-409E-8FA9-7461D7E39BDD |HDS       |Saturday         |
|253C8EB9-9406-4BA2-9C33-4279D70CF2B3 |2E2170B2-10F1-409E-8FA9-7461D7E39BDD |HDH       |Holiday Type     |
|49317973-D40A-4B97-A355-630001B3F980 |2E2170B2-10F1-409E-8FA9-7461D7E39BDD |HDSUN     |Sunday           |

The `entityMetadata` to add a table `EntityMetadata` is the show below: 

|EntityMetadataId                       | Code        | Name       | 
|---------------------------------------|-------------|------------|
|2E2170B2-10F1-409E-8FA9-7461D7E39BDD   | HDAY        |HolidaysType|


Each record of PTO have a status, and this status can be: 
- `denied `
- `approved`
- `requested` 
- `cancelled` 

The database must by filled with this data, And The table Type generic will by filled with the data on the table belove: 

|TypeGenericId                       |EntityMetadataId                    |Code        |Name                |
|------------------------------------|------------------------------------|------------|--------------------|
|C62FC6DC-47C0-4BA2-B765-0210957EBEA6|C9B2084C-FA79-4A2C-B09E-18702FD45958|PTODenied   |PTO Status Denied   |
|03C76016-1A1B-4396-AF10-60FC047FA958|C9B2084C-FA79-4A2C-B09E-18702FD45958|PTOApproved |PTO Status Approved |
|461A9258-18BE-40DF-8810-C5D6F829B0DC|C9B2084C-FA79-4A2C-B09E-18702FD45958|PTORequested|PTO Status Requested|
|F894A68F-6041-41FD-9804-F3555B27275B|C9B2084C-FA79-4A2C-B09E-18702FD45958|PTOCancelled|PTO Status Cancelled|

And the table EntityMetadata must be filled with the data above: 

|EntityMetadataId                    |Code|Name     |
|------------------------------------|----|---------|
|C9B2084C-FA79-4A2C-B09E-18702FD45958|PTOS|PTOStatus|



## Diagram for UI and tables for database 

In the fallowing link, there are diagram of all tables and UI schema for PTO functionality [Diagram](https://drive.google.com/file/d/1rF6A6RfcLrSGB9jnIwXzRwkg3i550TQN/view?usp=sharing). And at this [link](https://docs.google.com/document/d/1dwOrAJLlCWywvcRD2bLGBPMKCXONuuAg-PDTiwGuPoY/edit) there are a requirements and PBI for details about PTO functionality. 