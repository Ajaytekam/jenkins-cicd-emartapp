# emart-app

## Technology used   

* Nginx  
* Angular  
* NodeJS  
* Java  
* MongoDB  
* MySQL 

## Application Architecture 

```
                     ┌───────────────────┐
                     │Nginx (API Gateway)│
                     └───────────────────┘   
        ______________/        |        \____________
        |                      |                    |
        | "/"                  | "/api"             | "/webapi" 
        |                      |                    |
        ↓                      ↓                    ↓
┌─────────────────┐  ┌────────────────────┐  ┌─────────────────┐
│ Client(Angular) |  | NodaJS (Emart API) |  | Books API (java)│
└─────────────────┘  └────────────────────┘  └─────────────────┘
                               |                    |      
                               ↓                    ↓
                      ┌────────────────┐      ┌──────────────┐
                      |MongoDB Database|      |Mysql Database│
                      └────────────────┘      └──────────────┘
```


## Steps   

- 1. Fetch source code from git repository
- 2. Create Dockerfile for Client app, NodeJs api server, java api server, mongodb and mysql service, 
- 3. Create the docker-compose file, build image, test the services, and push the docker images onto dockerhub


## Commands to Initiate 

```
docker-compose up
```  




