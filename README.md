# How to Make a Skript Addon

## Preface:
This tutorial assumes you are knowledgable in java, and already know to to setup a minecraft plugin.

## Setting up the addon

### Maven
You will need to add the SkriptLang api to your maven file via the Skript Repository  
```xml
<repository>
    <id>skript-releases</id>
    <name>Skript Repository</name>
    <url>https://repo.skriptlang.org/releases</url>
</repository>
```
dependency:
```xml
<dependency>
    <groupId>com.github.SkriptLang</groupId>
    <artifactId>Skript</artifactId>
    <version>2.7.3</version> <!-- This should be set to the latest version. as of making this, it is 2.7.3; see https://repo.skriptlang.org/#/releases -->
    <scope>provided</scope>
</dependency>
```

### Main Plugin File
In the main plugin file for your skript addon you will need to register the addon for skript.
In the onEnable() function, you will need to add this code
(You will need to add the import for ch.njol.skript.Skript and ch.njol.skript.SkriptAddon)
```java
SkriptAddon addon = Skript.registerAddon(this); // you can also change this to `addon = Skript.registerAddon(this);` and store the addon instance outside of the onEnable() function
try {
    addon.loadClasses("me.symmettry.defaultskriptaddon"); // this should be set to your main package directory. for me i have it at me.symmettry.defaultskriptaddon, but put it to what you have
} catch (IOException e) {
    e.printStackTrace();
}
```
## Creating syntax

### Effects
An effect is simply; a piece of code that does not have a return value.
`send "hello" to player` is considered an effect.

First we will create a new `effects` package, and create a new class `EffEffectExample.class` under said package. 
You can provide Skript with extra information about your syntax using the `@Name` and `@Description` annotations, these will be used in documentation.
```java
@Name("Example")
@Description({"Example effect"})
```

Now our class will extend ch.njol.skript.lang.Effect
```java
package me.myself.skriptaddon.effects

public class EffExampleEffect extends Effect {
...
}
```

Inside of the class:
```java
// register the effect & syntax
static {
    Skript.registerEffect(EffExampleEffect.class, // change this to your file name.class
        "example effect" // This is the syntax, this will be used in your .sk file; when writing a script.
    );
}

// initiate effect
@Override
public boolean init(Expression<?>[] expr, int i, Kleenean kleenean, SkriptParser.ParseResult parseResult) {
    // returns true if we want Skript to register our effect.
    return true;
}

// stringify
@Override
public String toString(Event event, boolean b) {
    return "example effect";
}

// Main execution
@Override
protected void execute(Event event) {
    // Skript will call this method and execute the code in this method.
    Bukkit.getLogger().info("Test");
}
```
