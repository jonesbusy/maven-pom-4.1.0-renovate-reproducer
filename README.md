# renovatebot/renovate discussion 41734: Maven POM model version 4.1.0 is ignored

## Current behavior

This repository has two identical Maven projects. The only difference between them is the POM model version:

| File                    | `xmlns`                            | `<modelVersion>` | Detected by Renovate |
| :---------------------- | :--------------------------------- | :--------------- | :------------------- |
| `model-4.0.0/pom.xml`   | `http://maven.apache.org/POM/4.0.0` | `4.0.0`          | Yes                  |
| `model-4.1.0/pom.xml`   | `http://maven.apache.org/POM/4.1.0` | `4.1.0`          | **No**               |

Both depend on `org.apache.commons:commons-lang3:3.12.0`.

Renovate only extracts `model-4.0.0/pom.xml`:

- the Dependency Dashboard only lists `model-4.0.0/pom.xml` under the `maven` manager
- the `commons-lang3` update PR only changes `model-4.0.0/pom.xml`

`model-4.1.0/pom.xml` is silently skipped. Projects that use the Maven 4 POM model (`4.1.0`) get no Maven dependency updates.

The cause seems to be `parsePom` in `lib/modules/manager/maven/extract.ts`. It only accepts a POM when `xmlns` is `http://maven.apache.org/POM/4.0.0` or `<modelVersion>` is `4.0.0`.

## Expected behavior

Renovate also extracts dependencies from POMs that use model version `4.1.0` (Maven 4), so `model-4.1.0/pom.xml` is listed on the Dependency Dashboard and updated in the same `commons-lang3` PR.

## Link to the Renovate issue or Discussion

https://github.com/renovatebot/renovate/discussions/41734
