# Cheatsheet

This contains common scripts which could be used with Frida for various purposes.

For each script, I'll try and link to a page detailing more about what is going on.

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