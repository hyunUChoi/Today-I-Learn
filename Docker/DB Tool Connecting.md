$ docker exec -it [names] bash  
/# sqlplus '/as sysdba'  

```
user-name : system
password : oracle
```

create user [userName] identified [pw];  
grant connect, resource to [userName];
