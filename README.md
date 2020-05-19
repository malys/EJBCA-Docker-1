# EJBCA-Docker (Customizable) Dockerfile

## Why do this?
 Mainly because I loved EJBCA but didn't like the fact that I couldn't modify thier dockerfile to fit my needs.  They don't have one published as far as I could tell.
_____

### **IMPORTANT**
You MUST catch the output the first time the container is ran because the download location and password are provided.  Once it has been ran one time you no longer need to catch the output (provided you didn't remove the volume that's created)

In order to run the container just run `docker-compose up`.  That will start the container with the basic settings and you can access your CA using the URL https://myca.  

### Customize the container

In order to customize the CA go to the pki.env and db.env files and modify the data accordingly then run the above 'docker-compose up' command again

### db.env
POSTGRES_DB=ejbca ```[default entry to modify]```  
POSTGRES_USER=ejbca ```[default entry to modify]```  
POSTGRES_PASSWORD=ejbca ```[default entry to modify]```  
POSTGRES_INITDB_ARGS='--encoding=UTF-8'  

### pki.env
TLS_SETUP_ENABLED=false  
ORGANIZATION_UNIT=IT  
ORGANIZATION=Umbrella Organization  
STATE=SOMEWHERE  
COUNTRY=SOMEWHERE  
PRODUCTION=true  
DYNCONFIG=true  
SUPER=CAADMIN <-- THIS IS THE ADMINISTRATOR OF THE CA 
LANGS=eng  
DOCS=https://www.ejbca.org/docs  
ERRORNOTIFY=There was an error with the application  
LANG=eng  
CANAME=EvilCorpCA  
EMAIL=user@example.com  
DATABASE_JDBC_URL=jdbc:postgresql://db/ejbca  
DATABASE_USER=ejbca  **```[if you changed the entry above this entry MUST match that one]```**  
DATABASE_PASSWORD=ejbca  **```[if you changed the entry above this entry MUST match that one]```**
