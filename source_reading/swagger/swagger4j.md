# Swagger4J Source code reading

[swagger4j](https://github.com/SmartBear/swagger4j.git)

## Swagger4J Top to Button

- Factory and Build Pattern
- Interface Designed for entire Swagger Model
- Different Swagger Reader/writer/store
  
## Swagger Factory And Builder

- Swagger/SwaggerFactory
  * createResourceListing
  * createSwaggerReader
  * createSwaggerWriter
  * createApiDeclaration

## Interface Design
- ResourceListing
  * Apideclaration
  * Authorizations
  * path
- Api
  * ApiDeclaration
  * Operation
- DataType
- HasDataType
  * Model
  * RefArrayType
  * RefDataType
- Operation
- Parameter
- ResponseMessage

## Swagger Parser/Generator

Parse xml/json swagger def or create swagger def.