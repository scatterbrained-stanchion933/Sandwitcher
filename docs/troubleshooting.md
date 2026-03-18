# Troubleshooting

Common issues and how to fix them.

## Hook doesn't seem to fire

A few things to check:

- Make sure you called `Sandwitcher.init()` before `hook()`. If you didn't, it throws an `IllegalStateException`, but if you caught it somewhere upstream you might miss it.

- Make sure the method signature is exact. `getDeclaredMethod("foo", Int::class.java)` and `getDeclaredMethod("foo", Integer::class.java)` are different things on Android. Primitives and their boxed types have different `Class` objects.

- If you're hooking a method by class name, make sure the class is actually loaded by the time you hook it. If it's in a dynamic feature module or a plugin that hasn't loaded yet, `Class.forName` will throw.

- The method might be getting inlined by the JIT before your hook is installed. Sandwitcher disables JIT inlining on startup, but if you're calling `init()` late, some methods might already be inlined. Initialize as early as possible -- `Application.onCreate()` is the right place.

## App crashes on init

If you get a native crash during `Sandwitcher.init()`, it's usually one of:

- The device's ART implementation is heavily modified by the OEM. Some Chinese OEM ROMs strip or modify internal ART structures that Pine depends on. Not much you can do here other than test on that specific device.

- There's a conflict with another hooking library. If you're also using Xposed, Frida, or another ART hooker in the same process, they can step on each other.

## NoSuchMethodException

Double-check your method name and parameter types. Common mistakes:

```kotlin
// wrong: kotlin.Int is not the same as java int
getDeclaredMethod("setAge", Int::class.java)         // primitive int
getDeclaredMethod("setAge", Integer::class.java)      // boxed Integer

// wrong: using getMethod instead of getDeclaredMethod
// getMethod only finds public methods, getDeclaredMethod finds everything
getMethod("privateMethod")            // throws
getDeclaredMethod("privateMethod")    // works
```

## ClassNotFoundException with string-based hook

If you're using `Sandwitcher.hook(className = "...", ...)` and getting `ClassNotFoundException`, the class might not be loaded yet, or the name might be wrong.

For inner classes, use `$` notation:

```kotlin
Sandwitcher.hook(
    className = "com.example.Outer\$Inner",
    methodName = "doStuff",
    callback = myCallback
)
```

## Logcat not showing Sandwitcher logs

On Samsung devices (and some others) with `user` builds, `Log.d` and `Log.i` are suppressed. Sandwitcher uses `Log.w` for this reason, but if you're using a custom logcat filter, make sure you're including warning level.

Filter with:

```
adb logcat | grep Sandwitcher
```

## Hook works but unhook doesn't restore original behavior

Make sure you're calling `unhook()` on the right handle. If you hooked the same method multiple times, each `hook()` call returns a separate handle. Unhooking one doesn't remove the others.

If you want to remove everything, call `Sandwitcher.reset()`.
