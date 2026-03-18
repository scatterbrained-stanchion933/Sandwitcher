# Getting Started

This walks you through adding Sandwitcher to your project and hooking your first method.

## Add the dependency

In your app's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("io.sandwitcher:sandwitcher:0.1.0")
}
```

Make sure you have `mavenCentral()` in your repositories.

## Initialize

You need to call `Sandwitcher.init()` before hooking anything. Best place is your Application class:

```kotlin
class MyApp : Application() {
    override fun onCreate() {
        super.onCreate()
        Sandwitcher.init(this)
    }
}
```

Don't forget to register it in your manifest:

```xml
<application android:name=".MyApp" ... >
```

If you want to see what Sandwitcher is doing internally, pass debug config:

```kotlin
Sandwitcher.init(this, SandwitcherConfig(debugLogging = true))
```

This logs hook installs and removals to logcat under the `Sandwitcher` tag.

## Hook a method

Say you want to intercept `URL.openConnection()`. Get the method via reflection and hook it:

```kotlin
val method = URL::class.java.getDeclaredMethod("openConnection")

val handle = Sandwitcher.hook(method, object : HookCallback {
    override fun beforeMethod(param: MethodHookParam): HookAction {
        val url = param.thisObject as URL
        Log.d("MyApp", "Opening connection to ${url.host}")
        return HookAction.Continue
    }
})
```

That's it. Every call to `URL.openConnection()` in your app now goes through your callback first.

## Modify behavior

You can change arguments, swap out the return value, or skip the original method entirely.

Change the return value after the method runs:

```kotlin
override fun afterMethod(param: MethodHookParam) {
    // double whatever the method returned
    val original = param.result as Int
    param.result = original * 2
}
```

Skip the original method and return something else:

```kotlin
override fun beforeMethod(param: MethodHookParam): HookAction {
    return HookAction.ReturnEarly("my fake result")
}
```

Modify arguments before they reach the original:

```kotlin
override fun beforeMethod(param: MethodHookParam): HookAction {
    param.args[0] = "replaced argument"
    return HookAction.Continue
}
```

## Remove the hook

Hold onto the `HookHandle` returned by `hook()` and call `unhook()` when you're done:

```kotlin
handle.unhook()
```

The original method goes back to normal. You can check `handle.isActive` if you need to know whether the hook is still in place.

## Hook by class name

If you don't have the target class at compile time, use the string-based overload:

```kotlin
val handle = Sandwitcher.hook(
    className = "com.thirdparty.sdk.Analytics",
    methodName = "trackEvent",
    parameterTypes = arrayOf(String::class.java),
    callback = myCallback
)
```

This resolves the class at runtime via `Class.forName`, so it works even if the class isn't in your dependencies.
