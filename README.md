# How to Make a Skript Addon
This tutorial assumes you know the basics of Java and already know how to make a JavaPlugin.
Note: I just made this, so I don't have much included yet, only basic basic stuff.

## Setting up the addon

### Maven
You will need to add this repository:
```xml
<repository>
    <id>skript-releases</id>
    <name>Skript Repository</name>
    <url>https://repo.skriptlang.org/releases</url>
</repository>
```
And then this dependency:
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
To make an effect (an effect is something like "ban player") you will first need to create a file
It's good practice to sort and organize your code, so you should probably make an "effects" package.

Now, with a file named e.g. EffExampleEffect we can begin coding the effect

Before you start with `public class`, you can define some information for the effect such as:
```java
@Name("Example")
@Description({"Example effect"})
```

You need to make your class extend ch.njol.skript.lang.Effect
```java
public class EffExampleEffect extends Effect {
...
}
```

Inside of the class:
```java
// register the effect & syntax
static {
        Skript.registerEffect(EffExampleEffect.class, // change this to your file name.class
                "example effect" // this is syntax. it's formatted practically the same way was something like SkriptHub or any docs
        );
}

// initiate effect
@Override
public boolean init(Expression<?>[] expr, int i, Kleenean kleenean, SkriptParser.ParseResult parseResult) {
    return true;
}

// stringify
@Override
public String toString(Event event, boolean b) {
    return "example effect";
}

// Now, the main part. execution
@Override
protected void execute(Event event) {
    // you can run any code here now!
    Bukkit.getLogger().info("Test");
}
```
