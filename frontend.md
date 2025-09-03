# Frontend

   # --- Frontends ---
 
  
microfrontend1:
 
    
build:
 
./microfrontend1
 
    
ports:
       - "3001:3000"
 
 
  
microfrontend2:
 
    
build:
 
./microfrontend2
 
    
ports:
       - "3002:3000"
 
 
  
frontend-main:
 
    
build:
 
./frontend-main
 
    
ports:
       - "3000:80"
 
    
depends_on:
 
      
-
 
microfrontend1
 
      
-
 
microfrontend2
 
 
volumes:
 
  
mysql_data:
   mongo_data:
 
 
Como
 
rodar
 
 
# subir todos os containers
 
docker-compose
 
up
 
-d
 
--build
 
 # verificar se estão rodando
 docker ps
 
 
Frontend
 
principal:
 
http://<IP_VM>:3000
 
 
👉
 
Esse
 
docker-compose.yml
 
já
 
deixa
 
tudo
 
pronto
 
para
 
demonstrar
 
a
 
arquitetura
 
completa
,
 
mas
 
leve
 
o
 
bastante
 
para
 
rodar
 
numa
 
VM
 
OCI.
 
 
 
 
Frontend
 
●
 
Micro
 
Frontend
 
1
 
(Aluno)
:
 
Matrícula,
 
consulta
 
de
 
notas,
 
histórico.
 
●
 
Micro
 
Frontend
 
2
 
(Professor/Coord.)
:
 
Lançar
 
notas,
 
criar
 
cursos,
 
gerenciar
 
professores.
 
●
 
App
 
Integrador
:
 
Junta
 
os
 
micro
 
frontends
 
em
 
uma
 
única
 
aplicação
 
para
 
o
 
usuário
 
final.
 
 
 
Resumo
 
visual
 
do
 
fluxo
 
1.
 
Usuário
 
→
 
Login
 
no
 
Frontends
:
 
○
 
frontend-main
 
integra
 
os
 
microfrontends
 
○
 
microfrontend-1
 
e
 
microfrontend-2
 
→
 
pods
 
separados
 
4.
 
Gateway
:
 
faz
 
roteamento
 
para
 
os
 
microserviços
 
5.
 
Kubernetes
 
não
 
é
 
necessário
 
aqui,
 
Docker
 
Compose
 
já
 
organiza
 
bem
 
para
 
demo
 
6.
 
Portas
:
 
você
 
pode
 
mapear
 
cada
 
container
 
para
 
uma
 
porta
 
externa
 
da
 
VM
 
para
 
acessar
 
via
 
navegador
 
 
Dica
 
para
 
apresentação:
 
●
 
Mostre
 
o
 
diagrama
 
primeiro,
 
explique
 
que
 
na
 
produção
 
poderia
 
usar
 
Kubernetes
 
em
 
múltiplos
 
nós
.
 
●
 
Depois,
 
mostre
 
que
 
na
 
demo
 
você
 
rodou
 
tudo
 
em
 
1
 
VM
 
ARM
 
Free
 
Tier
,
 
mantendo
 
custo
 
zero
 
e
 
arquitetura
 
funcional.
 
 
Estrutura
 
de
 
Pastas
 
–
 
University
 
App
 
university-app/
 
│
 
├──
 
backend/
                      
#
 
Serviços
 
de
 
negócio
 
(Java
 
/
 
Spring
 
Boot)
 
│
   
├──
 
user-service/
              
#
 
Gerenciamento
 
de
 
usuários
 
│
   
├──
 
course-service/
            
#
 
Gerenciamento
 
de
 
cursos
 
│
   
├──
 
subject-service/
           
#
 
Gerenciamento
 
de
 
disciplinas
 
│
   
├──
 
classroom-service/
         
#
 
Gerenciamento
 
de
 
turmas
 
│
   
├──
 
enrollment-service/
        
#
 
Matrículas
 
│
   
└──
 
grade-service/
             
#
 
Notas
 
│
 
├──
 
infra/
                        
#
 
Infraestrutura
 
base
 
│
   
├──
 
keycloak/
                 
#
 
Autenticação
 
e
 
autorização
 
│
   
├──
 
mysql/
                    
#
 
Banco
 
de
 
dados
 
do
 
Frontend
 
(página
 
Home)
 
 
http://100.0.0.0/university-app/page/home
 
 
