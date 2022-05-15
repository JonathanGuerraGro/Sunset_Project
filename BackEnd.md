## pasos para este proyecto 💻

<p> Para este proyecto estamos contemplando que ya tienes instalado NPM.👨‍💻

- npm ``8.3.1``
- node ``16.14.0``
- Prisma:``3.14.0``
- eslint:``8.15.0``
- Postgres ``14.3 ``


## iniciando packet.json
`` npm init  ``
## instalando expres 
``npm install express --save-dev``

## instalando prisma
``npm install prisma --save-dev``

## iniciando prisma 
``npx prisma init``

## creando la base de datos con postgres
``create database Sunset;``



## creando model en prisma 

<p> Este codigo es el que ara la migracion con psrisma.

```
model Usario {
  id Int @id @default(autoincrement())
  Nombre String @unique
  Apellido String @db.VarChar(255)
  Correo String @db.VarChar(255)
  Telefono Int
  Kilos Int
  Playa String @db.VarChar(255)
  Estado String @db.VarChar(255)
  dateCreated DateTime @default(now())
  lastUpdated DateTime @updatedAt
}

```


## Informacion de la base de datos. 💻
| Campo      	| Tipo de Dato 	|
|------------	|:------------:	|
| Nombre     	| integer      	|
| Apellido   	| varchar      	|
| Correo     	| varchar      	|
| Telefono   	| int          	|
| Kilogramos 	| int          	|
| Playa      	| varchar      	|
| Estado     	| varchar      	|





## creando migrations de prsima 
<p> iniciamos  la migracion con el siguiente codigo.

``npx prisma migrate dev --name init ``


## creando 1 Seed.js
<p> creamos   seed.js para subir el primer usuario

```
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

(async function main() {
  try {
    const usuario = await prisma.usario.upsert({
      where: { Nombre: 'usuario' },
      update: {},
      create: {
        Nombre: 'usuario',
        Apellido: 'usuario',
        Correo: 'usuario@sunset.com',
        Telefono: 22222222,
        Kilos: 10,
        Estado: 'Chachalacas',
        Playa: 'Veracruz',
        
      },
    });

    
    console.log('Creando 1 usuario');
  } catch(e) {
    console.error(e);
    process.exit(1);
  } finally {
    await prisma.$disconnect();
  }
})();

```
## Creando server.js

<p> Creamos el server para Exxpress  Posteriormente  aremos los end points 


```
const express = require('express');
const app = express();
app.use(express.json());
const port = process.env.PORT || 3000;

// Require para usar Prisma
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

app.get('/', (req, res) => {
  res.json({message: 'alive'});
});

app.listen(port, () => {
  console.log(`Listening to requests on port ${port}`);
});

```

## End points  usados 🔘

| End points                       	|        Tipo de DatoRequest       	|        	| Response                                          	|
|----------------------------------	|:--------------------------------:	|--------	|---------------------------------------------------	|
| http://localhost:3000/usuario    	| http://localhost:3000/usuario    	| GET    	| Regresa a todos los usuarios                      	|
| http://localhost:3000/usuario/id 	| http://localhost:3000/usuario/id 	| GET    	| Regresa  el usuario con su id de la base de datos 	|
| http://localhost:3000/usuario    	| http://localhost:3000/usuario    	| POST   	| Crea un registro del usuario                      	|
| http://localhost:3000/usuario/id 	| http://localhost:3000/usuario/id 	| UPDATE 	| Modifica al usuario  reconocido por su id         	|
| http://localhost:3000/usuario/id 	| http://localhost:3000/usuario/   	| DELATE 	| Elimina al usuario por su respectiva id           	|



## server GET
```
app.get('/usuario', async (req, res) => {
    const alluser =  await prisma.usario.findMany({});
    res.json(alluser);
  });

```

## server POST
```
 app.post('/usuario', async (req, res) => {
    const usario = {
        Nombre: req.body.Nombre,
        Apellido: req.body.Apellido,
        Correo: req.body.Correo,
        Telefono: req.body.Telefono,
        Kilos: req.body.Kilos,
        Playa: req.body.Playa,
        Estado: req.body.Estado,
     };
    const message = 'Usuario creado.';
    await prisma.usario.create({data: usario});
    return res.json({message});
  });

```

## server PUT en nombre

```
app.put('/usuario/:id', async (req, res) => {
    const id = parseInt(req.params.id);
  
    await prisma.usario.update({
      where: {
        id: id
      },
      data: {
        Nombre: req.body.Nombre
      }
    })

```
## Server Delate

```
 app.delete('/usuario/:id', async (req, res) => {
    const id = parseInt(req.params.id);
    await prisma.usario.delete({where: {id: id}});
    return res.json({message: "Eliminado correctamente"});
  });

```

## instalacion de cors💻

``npm install cors --save``


## codigo de cors 

<p> Con corspodremos integrar el back end con el frontEnd

```
 const cors = require("cors");
const corsOption={
    origin:"http://localhost:8081"
};
app.use(cors(corsOption));

 ```

