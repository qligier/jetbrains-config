My configuration and recommendation for JetBrains IDEs.

# Intellij IDEA

### Plugins

- [.ignore](https://plugins.jetbrains.com/plugin/7495--ignore): adds support for various .ignore files.
- [CodeGlance Pro](https://plugins.jetbrains.com/plugin/18824-codeglance-pro): displays a zoomed out overview or
  minimap similar to the one found in Sublime into the editor pane.
- [JPA Buddy](https://plugins.jetbrains.com/plugin/15075-jpa-buddy): helps developers work efficiently with Hibernate,
  EclipseLink, Spring Data JPA, Flyway, Liquibase,
  Lombok, MapStruct, and other related technologies in both Java and Kotlin.
- [Maven Dependency Checker](https://plugins.jetbrains.com/plugin/18525-maven-dependency-checker): checks if there are
  any new Maven project dependencies available.
- [PIT Mutation Testing](https://plugins.jetbrains.com/plugin/7119-pit-mutation-testing): adds a 'Run configuration'
  that allows to execute PIT within IDE.
- [SonarLint](https://plugins.jetbrains.com/plugin/7973-sonarlint): finds and fixes coding issues in real-time, flagging
  issues as you code, just like a spell-checker.

### Project configuration

- Enable actions on file save in `Settings → Tools → Actions on Save`. _Reformat code_, _Optimize imports_, _Run code
  cleanup_ are nice tasks to enable. The setting is per project.
- Configure the copyright template of the project in `Settings → Editor → Copyright`. You can also update the
  copyright on file save.
- Add custom TODO patterns in `Settings → Editor → TODO` to match what is used in your projects.

### Global configuration

- Enable sync in `Settings → Settings Sync` to have a backup or synchronize between computers.
- Configure the live templates in `Settings → Editor → Live Templates`. See the [dedicated page](live_templates.md) for
  my live templates.
- Configure the code inspections to your preferences in `Settings → Editor → Inspection`. You can also create simple
  inspections.
- Configure the default code/file generation of things you do often in `Settings → Editor → File and Code Templates`.
  _Includes/File Header_ for the new file Javadoc, _Code/Catch Statement Body|Declaration_, etc. Other settings are
  available in `Settings → Editor → Code Style → Java → Code Generation`.
- Configure the template for getter/setter/equals/hashcode generation. You need to be in a Java class with members,
  then `Right
  click → Generate… → (Getter|Setter|Getter and Setter|equals() and hashCode()|toString()) → Button […] next to the
  Template dropdown`. See [my templates].
  (templates.md).
