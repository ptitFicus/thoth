# Event sourcing [![travis-badge][]][travis] [![jar-badge][]][jar]

[travis]:               https://travis-ci.com/MAIF/scribe
[travis-badge]:         https://travis-ci.com/MAIF/scribe.svg?token=yQytm3eoBniFj9mCoKpy&branch=master
[jar]:                  https://bintray.com/maif-functional-java/maven/scribe-core/_latestVersion
[jar-badge]:            https://api.bintray.com/packages/maif-functional-java/maven/scribe-core/images/download.svg

<p align="center">
    <img src="scribe.png" alt="Scribe" width="300"/>
</p>

This repository provides tools to implement event sourcing in your application. 
 
These libs are based on : 
 * Vavr for functional stuff (immutable `List`, `Either`, `Future`)
 * Akka stream for reactive streams
 * jackson for json 
 * jooq to build query 
 * vertx for reactive postgresql database access  
 * Postgresql and kafka are the in production tested data stores 

## Modules 

 * `commons-event`: POJOs that represent the stored events. Can be used by consumers to parse events. 
 * `scribe-core`: APIs for event-sourcing 
 * `scribe-jooq`: A jooq simple implementation of the `scribe-core` APIs   
 * `scribe-jooq-async`: A jooq implementation of the `scribe-core` APIs using the `jooq-async-api`interface
 
## Things to know 

The vavr `Future` is used for async call (java `CompletionStage` is not user-friendly). 

The akka stream `Source` is used for stream processing. 

The vavr `Either` is used to handle business errors. The idea is to have three channels:  
 * it's ok: `Future(Right("Result"))` 
 * it's an error: `Future(Left("Bad request"))`
 * it's a failure: `Future.failed(CrashedException("Crap!"))`

`io.vavr.Tuple0` is used instead of `void` so everything can be an expression: 

```java
Tuple0 sideEffect() {
    println("I have done a side effect");
    return Tuple.empty();
}
```

## Documentations 

* [Event sourcing](./scribe-core/readme.md) : the documentation of the core components
* [Message structure](./commons-events/readme.md) : description of messages structure
* [Event sourcing with reactive postgresql](./scribe-jooq-async/readme.md) : wire all together with the reactive postgresql client

## Limits

* A single command can't currently modify multiple entities [see this issue](https://github.com/MAIF/scribe/issues/4)

## Development 

### Compile / Test 

```bash
sbt compile
```

```bash
docker-compose -f docker-compose.test.yml up 
sbt test
```

Test rerun on each changes 

```bash
sbt ~test
```
