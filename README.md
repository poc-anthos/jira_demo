# Jira demo manifests

##  Steps

1. Create a demo/trial license for JIRA Servicedesk (in JIRA website).

2. Create two CCE Storages as below

  - create a CCE Storage EVS (for Postgres)
     Example: jira-evs-postgres type: EVS => postgres.yaml

  - create a CCE Storage SFS (jira shared directory, used for cluster)
     Example: jira-sfs-shared       type: SFS => sfs-shared.yaml

3. Edit posgres.yaml file and specify postgres DB name, user and password

```  
  POSTGRES_DB: jira
  POSTGRES_USER: admin
  POSTGRES_PASSWORD: XXXX
```

5. Lauch the Postgres deployment, and create a specific DB for JIRA. 

5. In the Jira config map, specify Jira namespace, DB name, Jira Username and Password as below:

```
  ATL_JDBC_URL: jdbc:postgresql://postgres.<namespace>:5432/<database_name>
  ATL_JDBC_USER: admin
  ATL_JDBC_PASSWORD: <password>
  ATL_DB_DRIVER: org.postgresql.Driver
  ATL_DB_TYPE: postgres72
```


5. JIRA Deployment should start with only one POD. 
 
- Jira will take aproximately 3min to get ready

6. Access the JIRA URL, and configure the base settings such as  "License", "BaseURL", etc.

7. In the ingress resource, the sticky session should be enabled. Exemple with nginx ingress use these annotations:

```
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/affinity-mode: "persistent"
```

