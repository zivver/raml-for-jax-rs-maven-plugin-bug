# Bugreport for raml-jaxrs-maven-plugin

Running `mvn verify` fails because `first.json` references `second.json`. `second.json` was not copied to the plugin's temp space by the [`Context`](https://github.com/mulesoft/raml-for-jax-rs/blob/a2aab2ad662954845ccdd0b5012d32aeaa97c539/raml-to-jaxrs/core/src/main/java/org/raml/jaxrs/codegen/core/Context.java#L130) class like the other JSON schema(s).

## Workaround
Adding `second.json` to the `schemas` section of the RAML file ensures that the file is copied to temp space. This is not desired because it is hard to keep track of all JSON schemas recusively.

## Possible solutions
- Not using temp space and temp files. Inline schema definitions in the RAML should be handled by reference instead of using their already included contents.
- Let the `Context` class analyze recursive JSON schemas wrt to the source directory.
- Just copy all `*.json` files from source to temp space rather than only the ones referenced in the RAML.
