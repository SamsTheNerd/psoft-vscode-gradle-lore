# Sam's guide to the gradliest vscode
This guide will help you set up a vscode environment using gradle for working with java in psoft.

## Handy Tips (good to know for using vscode and this guide)
- Command Palette (CTRL + SHIFT + P or CMD + SHIFT + P for mac) opens a textbox you can use to make vscode do things
- `>Java: Clean Java Language Server Workspace` restarts your window and clears some java-y stuff. it fixes problems sometimes, especially when setting up your environment. - this requires the Java VSCode Extension, I suggest just getting the [whole java extension pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

## Setup
- Follow the same git instructions posted online
- Add the ["Gradle for Java" VSCode Plugin](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-gradle)
- In the command palette run `>Gradle: Create a Gradle Java Project`.
    - Select your hw00 folder (or whatever hw you're on - it may already be selected) Make sure this is the same folder that your `src` folder is in.
    - If it asks you what type of project to make choose **Application**
    - It'll ask Groovy or Kotlin, choose **Groovy**
    - It'll ask what testing you want, choose JUnit 4 - or Jupiter, idk it probably doesn't matter, we're rewriting the `build.gradle` later anyways.
    - It'll ask what the project's name is, something like hw00 is fine, whatever your folder is named
    - It'll ask what your package name is, this should be `hw0` (or whatever homework you're on, it's the `src/main/java/PACKAGE-NAME` though).
- You should have a lot of new folders in your directory. 
    - `App` - delete this, don't need it
    - The rest are just build stuff that you probably don't need to worry about - you should update your `.gitignore` to exclude these though.
- In your command line run `java --version`. if your version is Java 20 or newer you'll need to update gradle with:

```cmd
mac/linux: ./gradlew wrapper --gradle-version 8.5
windows: gradlew.bat wrapper --gradle-version 8.5
```
You can check [here](https://docs.gradle.org/current/userguide/compatibility.html) for what version of gradle you need for your java.

- Make a file called `build.gradle` in your hw00 folder with the contents from the `build.gradle` file in this repo.

- In `build.gradle`, change the class assigned to javaMainClass to your project's entrypoint (the class with a `public void main(String[] argv)` method)

You'll need to do this every time you make a new project folder

## How To Use

On windows you run gradle commands in the command line (not powershell) with `gradlew.bat <task> <args and whatnot>`. On mac and linux it's `./gradlew <task> <args and whatnot>`. For the rest of the guide I'm going to use the mac/linux syntax since I prefer it, but make sure you use the right one for your OS !!

- Running: `./gradlew run`
    - runs the main method with no args
- Running: `./gradlew run --args"your args go here"`
    - runs the main method with the provided arguments
- Testing: `./gradlew test`
    - runs all the tests in the test directory
- Coverage Report: `./gradlew jacocoTestReport`
    - generates a coverage report using jacoco

VSCode should also pop up with little run/debug buttons above your main methods, and green run triangles next to your tests (as well as showing up in the test tab). These are probably easier to use than the gradlew commands for most cases. 

## JavaFX

For JavaFX you'll need to use the [javafx build.gradle](https://github.com/SamsTheNerd/psoft-vscode-gradle-lore/blob/main/javafx--RENAME-THIS-FILE--build.gradle) or add the following to your existing build.gradle:

add `id 'org.openjfx.javafxplugin' version '0.1.0'` in plugins so that it looks something like

``` groovy
plugins {
    id "application"
    id 'org.openjfx.javafxplugin' version '0.1.0'
}
```

and 

```groovy
javafx {
    version = "22"
    modules = [ 'javafx.controls' ] 
}
```

You may need more modules than just `javafx.controls`, add them in there if so, it's just a comma separated array. 

If you're getting a "class file has wrong version" error verify that you have Java 21 using `java --version` and update if needed. Otherwise try downgrading javafx to version `17.0.1`.

There's some more info on javafx gradle setups here https://openjfx.io/openjfx-docs/#gradle

Make sure to set the `javaMainClass` in the build.gradle so that you can run your non-test code.