# Microservicos

   # --- Microserviços ---
   user
-service:
     build: ./user
-service
 
    
ports:
       - "8081:8081"
 
    
environment:
       SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306
/keycloak
       KEYCLOAK_URL: http://keycloak:8080
 
    
depends_on:
 
      
-
 
keycloak
 
      
-
 
mysql
 
 
  
course-service:
 
    
build:
 
./course-service
 
    
ports:
       - "8082:8082"
 
    
environment:
       SPRING_DATA_MONGODB_URI: mongodb://mongo:27017
/course_db
 
    
depends_on:
 
      
-
 
mongo
 
 
  
subject-service:
 
    
build:
 
./subject-service
 
    
ports:
       - "8083:8083"
 
    
environment:
       SPRING_DATA_MONGODB_URI: mongodb://mongo:27017
/subject_db
 
    
depends_on:
 
      
-
 
mongo
 
 
  
classroom-service:
 
    
build:
 
./classroom-service
 
    
ports:
       - "8084:8084"
 
    
environment:
       SPRING_DATA_MONGODB_URI: mongodb://mongo:27017
/classroom_db
 
    
depends_on:
 
      
-
 
mongo
 
 
  
enrollment-service:
 
    
build:
 
./enrollment-service
 
    
ports:
       - "8085:8085"
 
    
environment:
       SPRING_DATA_MONGODB_URI: mongodb://mongo:27017
/enrollment_db
 
    
depends_on:
 
      
-
 
mongo
 
 
  
grade-service:
 
    
build:
 
./grade-service
 
    
ports:
       - "8086:8086"
 
    
environment:
       SPRING_DATA_MONGODB_URI: mongodb://mongo:27017
/grade_db
 
    
depends_on:
 
      
-
 
mongo
 
   # --- Gateway ---
 
  
gateway:
 
    
build:
 
./gateway
 
    
ports:
       - "80:80"
 
    
depends_on:
       - user
-service
 
      
-
 
course-service
 
      
-
 
subject-service
 
      
-
 
classroom-service
 
      
-
 
enrollment-service
 
      
-
 
grade-service
 
Microserviços
 
1.
 
User
 
Service
 
●
 
Função
:
 
Gerencia
 
informações
 
dos
 
usuários
 
(alunos,
 
professores
 
e
 
coordenadores).
 
●
 
Integração
:
 
A
 
autenticação
 
e
 
autorização
 
são
 
centralizadas
 
no
 
