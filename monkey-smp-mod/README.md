# Mod development

The build follows Fabric API's modular structure. Every subproject is a Fabric mod with its own `fabric.mod.json`, main/client source sets, sources JAR, and Maven publication. Shared configuration lives in the root `build.gradle`.

- `monkey-smp-core`: existing entrypoints, mixins, and assets.
- `monkey-smp-features`: starter feature module that depends on `monkey-smp-core`.
- Root `monkey-smp`: aggregate mod that bundles all subproject JARs.

All modules share the version and group from `gradle.properties`.

Dependency and Loom plugin versions are managed in `gradle/libs.versions.toml`. All projects can use its `libs` aliases, such as `libs.fabric.api`.

Use JDK 25:

```sh
./gradlew projects
./gradlew build
./gradlew runClient
./gradlew runServer
./gradlew :monkey-smp-core:build
```

The bundled mod is in `build/libs`; individual module JARs are in each module's `build/libs`. Root development runs use `run`.

To add a module:

1. Create its directory, `build.gradle`, and `src/main/resources/fabric.mod.json` with a unique mod ID.
2. Include its name in `settings.gradle`. The root automatically bundles it.
3. Declare module dependencies in its `build.gradle` (see `monkey-smp-features`) and matching runtime dependencies in its `fabric.mod.json`.
4. Add the module ID to the root metadata's `depends` if it is required by the aggregate mod.

Project dependencies provide code at compile/development time. Fabric metadata declares runtime mod requirements. Sibling modules are bundled by the root, not nested inside one another.
