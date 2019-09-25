### Mongo-queries-3

Cargar el fichero __social.json__

      mongoimport --uri "mongodb+srv://neollob:<password>@cluster0-..." --collection social --drop --file social.json

- Obtener los usuarios que han hecho más comentarios y los que han hecho menos de entre todas las
entradas de la bd. (tip: usar la cláusula unwind para crear un documento por cada comentario de cada
post (que inicialmente estarían en un array dentro de el post correspondiente).

Menos comentarios

    db.social.aggregate([
      { $unwind : "$comments" },
      {"$group" : {
        _id:{name:"$comments.by.name"}, 
        comments:{$sum:1}
      }},
      { "$sort": { 
        "name": 1, 
        "comments": 1 
      }},
      { $limit : 10 }
    ])
Más comentarios

    db.social.aggregate([
      { $unwind : "$comments" },
      {"$group" : {
        _id:{name:"$comments.by.name"}, 
        comments:{$sum:1}
      }},
      { "$sort": { 
        "name": 1, 
        "comments": -1 
      }},
      { $limit : 10 }
    ])
