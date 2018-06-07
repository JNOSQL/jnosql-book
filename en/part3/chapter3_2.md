# Implementing a Document Driver

If your database of choice is not supported by the existing set of Diana drivers, you can implement a driver for a document database by creating implementation classes for five or six interfaces in the `org.jnosql.diana.api.document` package:

* `DocumentCollectionManager`  to provide the CRUD operations for the database. Additionally, it may make sense to create an extended version of this interface to implement to add hooks to querying capabilities of the specific database.
* `DocumentCollectionManagerAsync` to provide asynchronous versions of the above.
* `DocumentCollectionManagerFactory` and `DocumentCollectionManagerAsyncFactory` to provide generation of the collection managers. These interfaces can be implemented in the same class.
* `DocumentConfiguration` and `DocumentConfigurationAsync` implementations to cover the configuration of your driver for synchronous and async variants. Alternatively, one class can implement `UnaryDocumentConfiguration` to cover both cases when there is no functional difference.

## Getting Started

To begin implementing a driver as a Maven project, create a new Java 8 project that includes at least the following artifact dependencies, in addition to your database's dependencies:

```markup
<dependency>
  <groupId>org.jnosql.diana</groupId>
  <artifactId>diana-driver-commons</artifactId>
  <version>${jnosql-version}</version>
</dependency>
<dependency>
  <groupId>org.jnosql.diana</groupId>
  <artifactId>diana-core</artifactId>
  <version>${jnosql-version}</version>
</dependency>
<dependency>
  <groupId>org.jnosql.diana</groupId>
  <artifactId>diana-document</artifactId>
  <version>${jnosql-version}</version>
</dependency>
<dependency>
  <groupId>io.reactivex</groupId>
  <artifactId>rxjava</artifactId>
  <version>1.3.1</version>
</dependency>
<dependency>
  <groupId>javax.validation</groupId>
  <artifactId>validation-api</artifactId>
  <version>2.0.1.Final</version>
</dependency>
```

## `DocumentCollectionManager` Class

The document collection manager class is the logical starting point of the implementation, and will likely contain the bulk of your database-specific details. It handles the core CRUD functions of the driver as well as any additional querying or other features you would like to add. The methods to implement are:

* `DocumentEntity insert(DocumentEntity)` and `DocumentEntity insert(DocumentEntity entity, Duration ttl)`: Take a [`DocumentEntity`](../part2/chapter2_3_2.md), which is essentially a series of potentially-nested key/value pairs and a document-type string name, convert it to your database's storage format, insert it, and return the `DocumentEntity` with any additions, such as an auto-generated ID or other computed-on-insertion values. The Artemis layer will have done the work of converting the original Java object into this entity and will convert the updated version back for the return value.
* `DocumentEntity update(DocumentEntity)`: update an existing document in the database. This is similar to `insert`, and it can be assumed going in that the document already exists in the database when this is called.
* `List<DocumentEntity> select(DocumentQuery)`: select one or more documents by query. The deletion query uses Diana's object-based query implementation, which will have to be converted to your database's variant and evaluated to select the documents. More on this below. This method is used to select both individual entities and arbitrary collections.
* `void delete(DocumentDeleteQuery)`: delete one or more documents by query.
* `void close()`: close any open resources, if applicable.

### Document Type Names

In addition to the key/value pairs, `DocumentEntity` contains a collection name string, which is analagous to a table in a relational database and may map to a store or other concept in your document database.

### Document IDs

Unlike collection names, Diana does not have a distinct concept of a document ID field outside of the key/value contents. Instead, it is largely based on convention, with the `@Id` annotation in Artemis allowing the user to specify the ID field and using `_id` by default. If the targetted database has the concept of an external arbitrary ID, the driver can choose to either look for this `_id` field and use that value or to just use auto- or manually-generated IDs when storing. Diana will always use selection queries when trying to select an individual document, and so a query translation should take into account any special handling of ID fields.

### The Query Language

The Diana query language is implemented in the `org.jnosql.diana.api.Condition` class and uses common comparison and boolean operations. The most common way to implement the translation is via a recursive method to convert each component and return the result. For example, this method converts the query to a semantically-similar query language that uses JSON objects and arrays:

```java
private static final Set<Condition> NOT_APPENDABLE = EnumSet.of(IN, Condition.AND, Condition.OR);
private static JsonObject getCondition(DocumentCondition condition, JsonObject params) {
  Document document = condition.getDocument();

  if (!NOT_APPENDABLE.contains(condition.getCondition())) {
    params.put(document.getName(), document.get());
  }

  String name = document.getName();
  Object placeholder = document.get();
  switch (condition.getCondition()) {
    case EQUALS:
      return JsonObject.of(name, JsonObject.of("$eq", placeholder));
    case LESSER_THAN:
      return JsonObject.of(name, JsonObject.of("$lt", placeholder));
    case LESSER_EQUALS_THAN:
      return JsonObject.of(name, JsonObject.of("$lte", placeholder));
    case GREATER_THAN:
      return JsonObject.of(name, JsonObject.of("$gt", placeholder));
    case GREATER_EQUALS_THAN:
      return JsonObject.of(name, JsonObject.of("$gte", placeholder));
    case LIKE:
      return JsonObject.of(name, JsonObject.of("$like", placeholder));
    case IN:
      return JsonObject.of(name, JsonObject.of("$in", placeholder));
    case AND:
      return JsonObject.of("$and", JsonArray.of(document.get(new TypeReference<List<DocumentCondition>>() {
      }).stream().map(d -> getCondition(d, params)).filter(Objects::nonNull).toArray()));
    case OR:
      return JsonObject.of("$or", JsonArray.of(document.get(new TypeReference<List<DocumentCondition>>() {
      }).stream().map(d -> getCondition(d, params)).filter(Objects::nonNull).toArray()));
    case NOT:
      DocumentCondition dc = document.get(DocumentCondition.class);
      return JsonObject.of("$not", getCondition(dc, params));
    default:
      throw new IllegalStateException("This condition is not supported in this driver: " + condition.getCondition());
  }
}
```

## `DocumentCollectionManagerAsync` Class

Depending on your database, the asynchronous variant of the document collection manager may be significantly different from the synchronous variant; alternatively, it may make sense to simply wrap the synchronous variant with reactive/callback methods.

## `DocumentCollectionManagerFactory`/`Async` Classes

The `DocumentCollectionManagerFactory` and `DocumentCollectionManagerFactoryAsync` class or classes are responsible for creating the collection managers for a given database name. The database name is merely a string and can correspond to whatever the applicable concept within your database is, or be ignored entirely. The factory should have a constructor that will be used by the `DocumentConfiguration` class and the class should handle the translation of the general configuration \(such as user credentials and database path\) to the actual connection objects for your database, which it can then pass to the `DocumentCollectionManager` instances.

## `DocumentConfiguration`/`Async` or `UnaryDocumentConfiguration` Classes

The configuration object is the final entry point for applications using your driver. It provides two methods, each with async variants: `DocumentCollectionManagerFactory get()` to get a default configuration instance \(or to potentially attempt to search for a configuration file to use\) and `DocumentCollectionManagerFactory get(Settings settings)`, which takes a `org.jnosql.diana.api.Settings` object, which in turn is a `Map<String, Object>`. Your implementation should define known keys to look for here and use them when constructing the connection to the back-end database.

