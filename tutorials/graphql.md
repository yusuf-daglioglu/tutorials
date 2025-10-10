# GRAPHQL

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 GraphQL

Sadece tek 1 URL ve path içerisinden POST request'i ile çalışır. POST body'sinde atılan JSON, GraphQL formatındadır. GraphQL kendi içinde request'in hangi fonksiyonları backend'de çağıracağını kendi tetikler.

POST isteğinin içi içerisinden yönetildiği için GraphQL, web browser'larda da full-featured çalışır.

POST payload örneği:

```json
{
  "operationName": "GenerateToken",
  "query":"mutation { generateToken(input: { username: \"yourUsername\", password: \"yourPassword\" }) { accessToken refreshToken } }",
  "variables": {
    "key1": "value1",
    "key2": "value2"
  }
}
```

query'nin içindeki string JSON değildir. Kendine has JSON'a benzeyen bir formatı var. Bu sebeple rahat okuyabilmek ve yazmak için özel bir eklenti kullanmak şart.

"variables" kısmı key-value map olmak zorunda.

response örneği:

```json
{
  "data": {
    
      "any json object here": "anything here"
    
  },

  "errors": [
    {
      "message": "user not found",
      "locations": [
        {
          "line": 3,
          "column": 3
        }
      ]
    }
  ],
}
```

- Data ve errors bilgileri response'umuzun en üstündedir.
- 1 graphQL isteğinde birden fazla query/mutation varsa, birden fazla error ve/veya data içinde birden fazla data olabilir.
- errors kısmı standarttır. bu kısım client'ın syntax error'larını da kapsar, sunucunun 500 hatalarını da kapsar.
- warning diye bir alan/standart yok.

  fakat istersek 2 yerde "extension" diye bir alan döndürebiliriz:
  - error ile aynı seviyede
  - errors içerisinde her error için ayrı ayrı.
  
  bu extension alanında istediğimizi dönebiliriz. Genelde:
  - warning'ler
  - request/response ile ilgili meta bilgiler burada dönülüyor:
    - response-ID gibi
    - response date-time
    - custom warning/error code
    - trace-id

- "locations" alanı standart. Eğer client syntax hatası yapmış ise, request'teki hatalı olan satır bilgisini yazıyor.

## 📌 example 1

```graphql
# "MyComplexQuery" is the name of this request. on GraphQL we name it as "operation".
# operation name is not defined on server side.

# Whole request use 3 dynamic parameters which are given in the first line.
# We must define all dynamic parameters inside MyComplexQuery function.
# Otherwise GraphQL server will not process the request.

# Each dynamic parameter has dollar icon. Which means they should be
# define in request inside "variables" JSON object.

query MyComplexQuery($var1: String!, $var2: Int!, $var3: Boolean) {

  # "firstData" is the response field name.
  # if we don't write "firstdata" here, the response will be the default value from scheme.

  # "getData" is the graphql function name which triggers a function on backend.

  # When we call getData function we send "id" parameter as "var1".
  firstData: getData(id: $var1) {

    # return only below fields:
    id
    name
  }

  secondData: getMoreData(limit: $var2) {

    # return only below fields:
    id
    description
  }

  thirdData: getOptionalData(flag: $var3) {

    # return only below fields:
    id
    value
    country {
      name # return the country object only with name parameter.
    }
  }
}
```

VALUES OF REQUEST:

```json
{
  "var1": "xyz",
  "var2": 6,
  "var3": false
}
```

RESPONSE:

```json
{
  "data": {
    "firstData": {
      "id": "1",
      "name": "jack"
    },
    "secondData": {
      "id": "2",
      "description": "abc"
    },
    "thirdData": {
      "id": "3",
      "value": "value33",
      "country": {
          "name": "Germany"
        }
    }
  }
}
```

## 📌 example 2

```graphql
query { # this line is same with query MyQuery {
 generateToken(input: { username: "jack", password: "123" }) {
  accessToken
  refreshToken
 } 
}
```

On above example we can not use $username because have not wrap whole request with another function. The same request with dynamic parameters:

```graphql
query MyQuery ( $username: String! ) {
 generateToken(input: { username: $username, password: "123" }) {
  accessToken
  refreshToken
 } 
}
```

VALUES OF REQUEST:

```json
{
  "username": "jack"
}
```

## 📌 example scheme

```graphql

## 📌 this is comment.

## 📌 this example is not a full working example.

type Mutation {
    # function name: createMerchant
    # variable name: merchant
    # variable type: CreateMerchant
    # return value type: ReadMerchant
    createMerchant(merchant: CreateMerchant):ReadMerchant
}

type Query {
    getMerchants: [ReadMerchant] # array
    getMerchantById(id: ID): ReadMerchant
}

type ReadMerchant implements BaseEntity{ # inheritance
    name: String! # ! means: GraphQL will throw exception if it is null.
    createdAt: Date
    address: ReadAddress
    contacts: [ReadContact] # array
}

type ReadContact {
    id: ID
    email: String
}

type ReadAddress {
    id: ID
    city: String
}

interface BaseEntity {
    ipAddress: String
    createdAt: Date
}

enum CompanyType {
    INDIVIDUAL,
    COOPERATIVE
}
```