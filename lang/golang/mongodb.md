Golang and Mongodb
===

- connect
- crud

## Connect Database

```go
package database

import (
  "context"
  "fmt"
  "log"
  "time"

  "go.mongodb.org/mongo-driver/mongo"
  "go.mongodb.org/mongo-driver/mongo/options"
)

var (
  // DBClient is exposed db client
  DBClient *mongo.Client
  // DB used in this app
  DB *mongo.Database
)

// Connect a mongodb
func Connect(host, port string) *mongo.Client {
  clientOptions := options.Client().ApplyURI("mongodb://" + host + ":" + port)
  ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
  defer cancel()

  client, err := mongo.Connect(ctx, clientOptions)

  if err != nil {
    log.Fatal(err)
  }

  err = client.Ping(context.TODO(), nil)

  if err != nil {
    log.Fatal(err)
  }
  fmt.Println("Connected to MongoDB!")
  return client
}

// Disconnect mongodb
func Disconnect() {
  ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
  defer cancel()
  DBClient.Disconnect(ctx)
}

```



## Insert document

```go
// CreatePage add page to db
func CreatePage(c *fiber.Ctx) {
  loc := time.FixedZone("UTC+8", +8*60*60)
  var page Page
  page.Title = c.FormValue("title")
  page.Tag = c.FormValue("tag")
  page.Cate = c.FormValue("cate")
  page.Author = c.FormValue("author")
  page.Body = c.FormValue("body")
  page.ID = primitive.NewObjectID()
  page.CreatedAt = time.Now().In(loc)
  page.UpdatedAt = time.Now().In(loc)
  collection := database.DB.Collection("page")
  _, err := collection.InsertOne(context.TODO(), page)
  if err != nil {
    log.Fatalln(err.Error())
  }
  c.Redirect("/pages/show/" + page.ID.Hex())
}
```



## Read One Document

```go
// ShowPage according id
func ShowPage(c *fiber.Ctx) {
  //  id := c.Params("id")
  objID, _ := primitive.ObjectIDFromHex(c.Params("id"))
  filter := bson.M{"_id": objID}
  var page Page
  collection := database.DB.Collection("page")

  err := collection.FindOne(context.TODO(), filter).Decode(&page)
  if err != nil {
    log.Fatal(err)
  }

  e := strconv.FormatInt(page.UpdatedAt.Unix(), 10)

  if match := c.Get("If-None-Match"); match != "" {
    if strings.Contains(match, e) {
      c.Status(304)
      return
    }
  }

  //page.Body = string(blackfriday.Run([]byte(page.Body)))

  c.Fasthttp.Response.Header.Add("Etag", e)
  Render(c, "PageShow", page, page.Title)
}
```



## Get More Document

```go
// GetPages retrieve all pages
func GetPages(c *fiber.Ctx) {
  collection := database.DB.Collection("page")

  findOptions := options.Find()
  findOptions.SetSort(bson.M{"created_at": -1}).SetSkip(offset).SetLimit(10)

  var pages []Page

  // Passing bson.D{{}} as the filter matches all documents in the collection
  cur, err := collection.Find(context.TODO(), bson.D{{}}, findOptions)
  if err != nil {
    log.Fatal(err)
  }

  for cur.Next(context.TODO()) {
    var page Page
    err := cur.Decode(&page)
    if err != nil {
      log.Fatal(err)
    }
    pages = append(pages, page)
  }

  if err := cur.Err(); err != nil {
    log.Fatal(err)
  }
  // Close the cursor once finished
  cur.Close(context.TODO())
  RenderAdmin(c, "PageIndex", fiber.Map{"Data": pages})
}

```



## Update document

```go
// UpdatePage is
func UpdatePage(c *fiber.Ctx) {
  objID, err := primitive.ObjectIDFromHex(c.Params("id"))
  if err != nil {
    return
  }
  loc := time.FixedZone("UTC+8", +8*60*60)
  var page = fiber.Map{}
  page["title"] = c.FormValue("title")
  page["tag"] = c.FormValue("tag")
  page["cate"] = c.FormValue("cate")
  page["author"] = c.FormValue("author")
  page["body"] = c.FormValue("body")
  page["updated_at"] = time.Now().In(loc)
  collection := database.DB.Collection("page")
  _, err = collection.UpdateOne(context.TODO(), bson.M{"_id": objID}, bson.M{"$set": page})

  if err != nil {
    log.Fatalln(err.Error())
  }
  c.Redirect("/admin/pages/1")
}

```



## Operators 

https://docs.mongodb.com/manual/reference/operator/

- Query and projection operators
- Update operators
- Query Modifiers



## Delete document

```go
func DeletePage(c *fiber.Ctx) {
  objID, err := primitive.ObjectIDFromHex(c.Params("id"))
  if err != nil {
    return
  }

  collection := database.DB.Collection("page")

  _, err = collection.DeleteOne(context.TODO(), bson.M{"_id": objID})
  if err != nil {
    fmt.Println(err)
    return
  }
  c.Redirect("/admin/pages/1")
}

```

