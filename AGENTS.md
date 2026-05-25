# AGENTS.md

## Project at a glance
- This is a single-module Android app (`:app`) using the Android Gradle Plugin via version catalogs (`gradle/libs.versions.toml`).
- Language is currently Java-only in app code (`app/src/main/java/com/thei/petfr/MainActivity.java`).
- UI is XML-based with `ConstraintLayout` (`app/src/main/res/layout/activity_main.xml`), not Jetpack Compose.
- Current codebase is Android Studio template baseline (one activity, one layout, template tests).

## Architecture and flow
- Entry point is `MainActivity` declared as launcher in `app/src/main/AndroidManifest.xml`.
- Runtime flow today is: app launch -> `MainActivity.onCreate` -> `EdgeToEdge.enable(this)` -> inflate `activity_main` -> apply system bar insets to root view `@id/main`.
- There are no service/repository/data layers yet; all behavior is in the activity.
- App theme chain is `Theme.PET_FilamentRecycler` -> `Base.Theme.PET_FilamentRecycler` in `app/src/main/res/values/themes.xml`.

## Build and test workflows (verified)
- Build debug APK:
  - `./gradlew :app:assembleDebug`
- Run local unit tests:
  - `./gradlew :app:testDebugUnitTest`
- Run instrumented tests on connected device/emulator:
  - `./gradlew :app:connectedDebugAndroidTest`
- Install debug build to device:
  - `./gradlew :app:installDebug`
- Run Android lint for debug:
  - `./gradlew :app:lintDebug`

## Project-specific conventions to preserve
- Keep dependencies and plugin versions in `gradle/libs.versions.toml`; reference them with `libs.*` aliases in `build.gradle.kts` files.
- Keep module repositories centralized (`RepositoriesMode.FAIL_ON_PROJECT_REPOS` in `settings.gradle.kts`); do not add ad-hoc module-level repositories.
- Maintain Java 11 source/target compatibility unless the whole project is intentionally upgraded (`app/build.gradle.kts`).
- Keep package/application ID aligned (`com.thei.petfr` appears in `app/build.gradle.kts`, `AndroidManifest.xml`, and tests).
- For edge-to-edge screens, ensure root layout has an ID and apply insets similarly to `MainActivity` (`R.id.main`).

## Testing and quality baseline
- Current tests are template smoke tests only:
  - Local: `app/src/test/java/com/thei/petfr/ExampleUnitTest.java`
  - Instrumented: `app/src/androidTest/java/com/thei/petfr/ExampleInstrumentedTest.java`
- When adding features, place pure logic in testable classes (outside activities) and keep activities thin for easier unit coverage.

## Integration and environment notes
- Android SDK path is resolved from local machine config (`local.properties`); this file is machine-specific.
- Backup/data extraction XMLs are template placeholders (`app/src/main/res/xml/backup_rules.xml`, `app/src/main/res/xml/data_extraction_rules.xml`) and currently do not include app-specific rules.
- Gradle wrapper is pinned to `9.4.1` (`gradle/wrapper/gradle-wrapper.properties`); use `./gradlew` rather than a globally installed Gradle.

