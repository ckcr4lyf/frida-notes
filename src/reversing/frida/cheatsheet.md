# Cheatsheet

This contains common scripts which could be used with Frida for various purposes.

For each script, I'll try and link to a page detailing more about what is going on.

## Hooking a function

Log arguments and return value

```js
setTimeout(function(){
    Java.perform(function () {
        const className = `com.example.TestClass`;
        var clazz = Java.use(className);
        console.log(`Hooking ${className}`)
        clazz.testMethod.implementation = function () {
            console.log("\n>>> testMethod() called!");
            console.log(">>> Arguments: ", JSON.stringify(arguments)); // the arguments individually are [0], [1] etc.

            // Call the real implementation to get the real return value
            const retVal = clazz.testMethod.apply(this, arguments);
            console.log("<<< Return value", retVal);

            // Here we can return the "real" value, or stub it with something else as well
            return retVal;
        }
    });
}, 0);
```

## Hooking Dynamic Class Loaders

List all loaders

```js
setTimeout(function(){
    Java.enumerateClassLoaders({
        onMatch: function(loader){
            console.log(`>>> Received loader: ${loader}`);
        },
        onComplete: function(){

        }
    });
}, 0);
```

Hooking methods from a particular class [(thanks to serializethoughts)](https://serializethoughts.com/2021/05/07/frida-java-classloaders)

```js
setTimeout(function(){
Java.enumerateClassLoaders({
        onMatch: function(loader){
            Java.classFactory.loader = loader;
            var TestClass;

            // Hook the class if found, else try next classloader.
            try{
                TestClass = Java.use("com.example.TestClass");
                TestClass.testMethod.implementation = function(){
                    // Here you can stub it, log the arguments, change the return value etc.
                    console.log("[+] Inside test method");
                }
            }catch(error){
                if(error.message.includes("ClassNotFoundException")){
                    console.log("\n You are trying to load encrypted class, trying next loader");
                }
            }
        },
        onComplete: function(){

        }
    });

}, 0);

```