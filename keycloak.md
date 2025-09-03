# Keycloak

Keycloak)
 
●
 
mongo
 
(banco
 
único
 
para
 
os
 
serviços)
 
●
 
gateway
 
(Node.js
 
ou
 
Spring
 
Cloud
 
Gateway
 
ou
 
Nginx)
 
Front
 
●
 
frontend-main
 
(Nginx
 
servindo
 
os
 
microfronts
 
integrados)
 
●
 
microfrontend-1
 
●
 
microfrontend-2
 
Exemplo
 
de
 
docker-compose.yml
 
(simplificado
 
p/
 
demo)
 
version: '3.9'
 
services:
 
  
mysql:
     image: mysql:8.0
 
    
container_name:
 
mysql
 
    
restart:
 
always
 
    
environment:
 
      
MYSQL_ROOT_PASSWORD:
 
root
 
      
MYSQL_DATABASE:
 
keycloak
 
      
MYSQL_USER:
 
keycloak
 
      
MYSQL_PASSWORD:
 
keycloak123
 
    
ports:
       - "3306:3306"
 
    
volumes:
 
      
-
 
mysql_data:/var/lib/mysql
 
 
  
keycloak:
     image: quay.io/keycloak/keycloak:24.0
 
    
container_name:
 
keycloak
 
    
command:
 
start
 
--optimized
 
    
environment:
 
      
KC_DB:
 
mysql
       KC_DB_URL: jdbc:mysql://mysql:3306
/keycloak
 
      
KC_DB_USERNAME:
 
keycloak
 
      
KC_DB_PASSWORD:
 
keycloak123
 
      
KEYCLOAK_ADMIN:
 
admin
 
      
KEYCLOAK_ADMIN_PASSWORD:
 
admin123
 
    
ports:
       - "8080:8080"
 
    
depends_on:
 
      
-
 
mysql
 
 
  
mongo:
     image: mongo:6.0
 
    
container_name:
 
mongo
 
    
restart:
 
always
 
    
ports:
       - "27017:27017"
 
    
volumes:
 
      
-
 
mongo_data:/data/db
 
Keycloak:
 
http://<IP_VM>:8080
 
Gateway
 
(API
 
entrypoint):
 
http://<IP_VM>
 
Keycloak
 
(com
 
banco
 
MySQL).
 
●
 
Responsabilidades
:
 
○
 
Registrar
 
novos
 
usuários
 
(apenas
 
Aluno
 
pode
 
se
 
auto-registrar).
 
○
 
Consultar
 
dados
 
do
 
perfil
 
do
 
usuário.
 
○
 
Coordenador
 
pode
 
promover/criar
 
usuários
 
com
 
papel
 
de
 
Professor.
 
●
 
Banco
:
 
Keycloak
 
+
 
MySQL.
 
 
 
2.
 
Course
 
Service
 
●
 
Função
:
 
Gerenciar
 
os
 
cursos
 
da
 
faculdade
 
(ex:
 
Engenharia,
 
Administração).
 

●
 
Responsabilidades
:
 
○
 
Criar,
 
editar
 
e
 
desativar
 
cursos.
 
○
 
Associar
 
matérias
 
(subjects)
 
a
 
um
 
curso.
 
●
 
Banco
:
 
MongoDB
 
(coleção
 
de
 
cursos).
 
 
 
3.
 
Subject
 
Service
 
●
 
Função
:
 
Gerenciar
 
as
 
matérias
 
(disciplinas)
 
oferecidas
 
nos
 
cursos.
 
●
 
Responsabilidades
:
 
○
 
Criar/editar
 
matérias
 
(ex:
 
Matemática
 
I,
 
Programação
 
I).
 
○
 
Definir
 
pré-requisitos
 
(ex:
 
Programação
 
II
 
precisa
 
de
 
Programação
 
I).
 
○
 
Associar
 
matéria
 
a
 
um
 
curso.
 
●
 
Banco
:
 
MongoDB
 
(coleção
 
de
 
subjects).
 
 
 
4.
 
Classroom
 
Service
 
●
 
Função
:
 
Criar
 
e
 
gerenciar
 
salas
 
de
 
aula
 
para
 
uma
 
matéria
 
específica.
 
●
 
Responsabilidades
:
 
○
 
Professor
 
vinculado
 
à
 
sala
 
(quem
 
ministra).
 
○
 
Lista
 
de
 
alunos
 
matriculados
 
naquela
 
turma.
 
○
 
Status
 
da
 
sala
 
(aberta/fechada).
 
●
 
Banco
:
 
MongoDB
 
(coleção
 
de
 
classrooms).
 
 
 
5.
 
Enrollment
 
Service
 
●
 
Função
:
 
Gerenciar
 
as
 
matrículas
 
dos
 
alunos
 
em
 
matérias/turmas.
 
●
 
Responsabilidades
:
 
○
 
Verificar
 
pré-requisitos
 
antes
 
da
 
matrícula.
 
○
 
Validar
 
regra
 
de
 
no
 
máximo
 
5
 
matérias
 
por
 
semestre
.
 
○
 
Criar
 
vínculo
 
entre
 
aluno
 
→
 
sala
 
de
 
aula
 
(subject).
 
○
 
Controlar
 
liberação
 
de
 
nova
 
matrícula
 
somente
 
se
 
semestre
 
concluído.
 
●
 
Banco
:
 
MongoDB
 
(coleção
 
de
 
enrollments).
 
 
 
6.
 
Grade
 
Service
 
●
 
Função
:
 
Gerenciar
 
as
 
notas
 
dos
 
alunos
 
nas
 
matérias.
 
●
 
Responsabilidades
:
 
○
 
Professores
 
lançam
 
3
 
notas
 
por
 
matéria.
 
○
 
Calcular
 
média
 
automática.
 
○
 
Se
 
média
 
<
 
7
 
→
 
permitir
 
nota
 
de
 
recuperação.
 
○
 
Emite
 
relatório
 
de
 
conclusão
 
(aprovado/reprovado).
 
●
 
Banco
:
 
MongoDB
 
(coleção
 
de
 
grades).
 
 
Infra
 
de
 
apoio
 
🔑
 
Keycloak
 
(com
 
MySQL)
 
●
 
Função
:
 
Centralizar
 
login,
 
autenticação
 
e
 
controle
 
de
 
roles
 
(Aluno,
 
Professor,
 
Coordenador).
 
●
 
Banco
:
 
MySQL.
 
●
 
Papel
:
 
Sem
 
ele,
 
nenhum
 
serviço
 
aceita
 
requisições
 
(JWT
 
obrigatório).
 
 
 
🌐
 
API
 
Gateway
 
●
 
Função
:
 
Entrada
 
única
 
para
 
todos
 
os
 
microserviços.
 
●
 
Responsabilidades
:
 
○
 
Roteamento
 
para
 
o
 
serviço
 
correto.
 
○
 
Validação
 
do
 
token
 
JWT
 
emitido
 
pelo
 
Keycloak.
 
○
 
Pode
 
aplicar
 
rate
 
limiting
 
e
 
logs
 
centralizados.
 
 
 
🎨
 
Keycloak
 
→
 
recebe
 
token
 
JWT.
 
2.
 
Usuário
 
acessa
 
frontend
 
→
 
Gateway
 
→
 
valida
 
token
 
no
 
Keycloak.
 
3.
 
Gateway
 
redireciona
 
requisição
 
ao
 
microserviço
 
correto.
 
4.
 
Cada
 
microserviço
 
salva/consulta
 
dados
 
no
 
MongoDB
 
(exceto
 
Keycloak
 
no
 
MySQL).
 
Arquitetura
 
–
 
1
 
VM
 
 
Usar
 
1
 
VM
 
ARM
 
com
 
4
 
vCPU
 
+
 
24
 
GB
 
RAM,
 
é
 
executar
 
para
 
Docker
 
Compose
 
e
 
todos
 
os
 
serviços.
 
Se
 
quiser
 
mais
 
segurança
 
e
 
testes
 
paralelos,
 
poderia
 
dividir
 
em
 
2
 
VMs
 
ARM
 
de
 
2
 
vCPU
 
+
 
12
 
GB
 
RAM
 
cada,
 
mas
 
uma
 
VM
 
grande
 
é
 
mais
 
simples.
 
 
O
 
diagrama/visual
 
da
 
arquitetura
 
para
 
sua
 
demo
 
rodando
 
1
 
VM
 
ARM
 
de
 
4
 
vCPU
 
+
 
24
 
GB
 
RAM
 
na
 
Oracle
 
Cloud
 
Free
 
Tier.
 
Isso
 
ajuda
 
a
 
mostrar
 
para
 
recrutadores
 
ou
 
em
 
portfólio.
 
 
 
Observações
 
do
 
Diagrama
 
1.
 
Tudo
 
em
 
1
 
VM
:
 
todos
 
os
 
14
 
containers
 
rodam
 
juntos
 
via
 
Docker
 
Compose.
 
2.
 
Bancos
:
 
○
 
MySQL
 
→
 
usado
 
apenas
 
pelo
 
Keycloak
 
○
 
Mongo
 
→
 
usado
 
por
 
todos
 
os
 
microserviços
 

3.
 
Keycloak
 
│
   
├──
 
mongo/
                    
#
 
Banco
 
único
 
para
 
os
 
microserviços
 
│
   
└──
 
gateway/
                  
#
 
API
 
Gateway
 
(Nginx
 
ou
 
Node.js)
 
│
 
├──
 
front/
                        
#
 
Camada
 
de
 
apresentação
 
│
   
├──
 
frontend-main/
            
#
 
App
 
container
 
principal
 
(Nginx)
 
│
   
├──
 
microfrontend-1/
          
#
 
MFE1
 
│
   
└──
 
microfrontend-2/
          
#
 
MFE2
 
│
 
└──
 
docker-compose.yml
            
#
 
Orquestração
 
dos
 
serviços
 
Configuração
 
do
 
Gateway
 
(Nginx)
 
O
 
gateway
 
será
 
o
 
ponto
 
de
 
entrada
 
único
 
(
http://<ip>/university-app/...
).
 
 
Ele
 
roteia
 
requisições
 
para
 
backends,
 
Keycloak
 
e
 
frontend
.
 
📄
 
infra/gateway/nginx.conf
 
events
 
{}
 
 http
 
{
     server
 
{
         listen 80
;
 
         server_name
 
_;
 
         # -------------------------
         # BACKEND (API)
         # -------------------------
         location
 
/university-app/api/course/
 
{
             proxy_pass
 
http://course-service:8080/;
 
        
}
 
         location
 
/university-app/api/user/
 
{
             proxy_pass
 
http://user-service:8080/;
 
        
}
 
         location
 
/university-app/api/subject/
 
{
             proxy_pass
 
http://subject-service:8080/;
 
        
}
 
         location
 
/university-app/api/classroom/
 
{
             proxy_pass
 
http://classroom-service:8080/;
 
        
}
 
         location
 
/university-app/api/enrollment/
 
{
             proxy_pass
 
http://enrollment-service:8080/;
 
        
}
 
         location
 
/university-app/api/grade/
 
{
             proxy_pass
 
http://grade-service:8080/;
 
        
}
 
         # -------------------------
         # INFRA (Keycloak)
         # -------------------------
 
        location
 
/university-app/infra/keycloak/
 
{
             proxy_pass
 
http://keycloak:8080/;
 
        
}
 
         # -------------------------
         # FRONTEND (páginas)
         # -------------------------
         location
 
/university-app/page/
 
{
             root
 
/usr/share/nginx/html;
             index
 
index.html;
             try_files $uri
 
/index.html;
 
        
}
 
    
}
 }
 
 
📄
 
infra/gateway/Dockerfile
 
FROM nginx:1.25
-alpine
 
 # Remove config padrão
 RUN
 
rm
 
/etc/nginx/conf.d/default.conf
 
 # Copia config customizada
 COPY
 
nginx.conf
 
/etc/nginx/nginx.conf
 
 # Copia build do frontend principal
 COPY
 
./front/frontend-main/dist/
 
/usr/share/nginx/html/
 
 EXPOSE 80
 
Arquivo
 
docker-compose.yml
 
📄
 
docker-compose.yml
 
version: "3.9"
 
services:
   # -------------------
   # Infra
   # -------------------
 
  
mysql:
     image: mysql:8.0
 
    
container_name:
 
mysql
 
    
environment:
 
      
MYSQL_ROOT_PASSWORD:
 
root
 
      
MYSQL_DATABASE:
 
keycloak
 
      
MYSQL_USER:
 
keycloak
 
      
MYSQL_PASSWORD:
 
keycloak
 
    
ports:
       - "3306:3306"
 
    
networks:
 
      
-
 
university-net
 
 
  
mongo:
     image: mongo:6
 
    
container_name:
 
mongo
 
    
ports:
       - "27017:27017"
 
    
networks:
 
      
-
 
university-net
 
 
  
keycloak:
     image: quay.io/keycloak/keycloak:26.0.2
 
    
container_name:
 
keycloak
 
    
command:
 
start-dev
 
--hostname-strict=false
 
    
environment:
 
      
KC_DB:
 
mysql
       KC_DB_URL: jdbc:mysql://mysql:3306
/keycloak
 
      
KC_DB_USERNAME:
 
keycloak
 
      
KC_DB_PASSWORD:
 
keycloak
 
      
KEYCLOAK_ADMIN:
 
admin
 
      
KEYCLOAK_ADMIN_PASSWORD:
 
admin
 
    
ports:
       - "8081:8080"
 
    
depends_on:
 
      
-
 
mysql
 
    
networks:
 
      
-
 
university-net
 
 
  
gateway:
 
    
build:
 
./infra/gateway
 
    
container_name:
 
gateway
 
    
ports:
       - "80:80"
 
    
depends_on:
       - user
-service
 
      
-
 
course-service
 
      
-
 
keycloak
 
    
networks:
 
      
-
 
university-net
 
   # -------------------
   # Backend Services
   # -------------------
   user
-service:
     build: ./backend/user
-service
     container_name: user
-service
 
    
ports:
       - "8082:8080"
 
    
networks:
 
      
-
 
university-net
 
 
  
course-service:
 
    
build:
 
./backend/course-service
 
    
container_name:
 
course-service
 
    
ports:
       - "8083:8080"
 
    
networks:
 
      
-
 
university-net
 
 
  
subject-service:
 
    
build:
 
./backend/subject-service
 
    
container_name:
 
subject-service
 
    
ports:
       - "8084:8080"
 
    
networks:
 
      
-
 
university-net
 
 
  
classroom-service:
 
    
build:
 
./backend/classroom-service
 
    
container_name:
 
classroom-service
 
    
ports:
       - "8085:8080"
 
    
networks:
 
      
-
 
university-net
 
 
  
enrollment-service:
 
    
build:
 
./backend/enrollment-service
 
    
container_name:
 
enrollment-service
 
    
ports:
       - "8086:8080"
 
    
networks:
 
      
-
 
university-net
 
 
  
grade-service:
 
    
build:
 
./backend/grade-service
 
    
container_name:
 
grade-service
 
    
ports:
       - "8087:8080"
 
    
networks:
 
      
-
 
university-net
 
 
networks:
 
  
university-net:
     driver: bridge
 
Rotas
 
finais
 
●
 
Backend
 
Course
 
 
http://100.0.0.0/university-app/api/course/...
 
●
 
Backend
 
User
 
 
http://100.0.0.0/university-app/api/user/...
 
●
 
Keycloak
 
 
http://100.0.0.0/university-app/infra/keycloak/...
 
●
 
