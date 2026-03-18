# Advanced Usage

Some patterns and edge cases you'll run into as you use Sandwitcher for more than basic hooks.

## Hooking static methods

Works the same way. `param.thisObject` will be `null` since there's no instance.

```kotlin
val method = System::class.java.getDeclaredMethod("currentTimeMillis")

Sandwitcher.hook(method, object : HookCallback {
    override fun afterMethod(param: MethodHookParam) {
        // param.thisObject is null here
        Log.d("Time", "currentTimeMillis returned ${param.result}")
    }
})
```

## Hooking native methods

Also works. Pine/LSPlant handles native methods the same way it handles Java methods -- it swaps the entry point at the ART level, which sits above the native/Java distinction.

```kotlin
val method = System::class.java.getDeclaredMethod("currentTimeMillis")
// this is a native method, hooks fine
Sandwitcher.hook(method, myCallback)
```

## Hooking private methods

Use `getDeclaredMethod` (not `getMethod`) to find private methods. You don't need to call `setAccessible` -- Sandwitcher handles that internally.

```kotlin
val method = SomeClass::class.java.getDeclaredMethod("privateHelper", Int::class.java)
Sandwitcher.hook(method, myCallback)
```

## Multiple hooks on the same method

You can hook the same method more than once. Each hook runs independently. They fire in the order they were installed.

```kotlin
val method = URL::class.java.getDeclaredMethod("openConnection")
val handle1 = Sandwitcher.hook(method, loggingCallback)
val handle2 = Sandwitcher.hook(method, headerInjectionCallback)
```

Unhooking one doesn't affect the other.

## Calling the original from beforeMethod

If you want to call the original method yourself (e.g., to decide based on the result whether to proceed), you can't do that directly from `beforeMethod`. The intended pattern is:

- Let the original run (return `Continue`)
- Check the result in `afterMethod`
- Modify or replace it there

If you really need to skip the original conditionally, use `ReturnEarly` with whatever value makes sense for the caller.

## Thread safety

Hooks fire on whatever thread called the original method. Your callback code needs to handle that. If you're touching UI, use `runOnUiThread` or post to a handler.

The `MethodHookParam` instance is per-call and per-thread, so you don't need to worry about two threads stepping on each other's params.

`HookHandle.unhook()` is safe to call from any thread, and calling it more than once is fine -- the second call is a no-op.

## Error handling in callbacks

If your `beforeMethod` or `afterMethod` throws, Sandwitcher catches it and logs it. The original method still runs (or returns, if it already ran). Your exception won't crash the app or break the hook.

That said, don't rely on this as a pattern. If something can fail in your callback, handle it yourself.

## Hooking methods that throw

If the original method throws an exception, it shows up in `param.throwable` in your `afterMethod`. You can inspect it, log it, suppress it, or replace it:

```kotlin
override fun afterMethod(param: MethodHookParam) {
    if (param.throwable != null) {
        Log.e("Hook", "Method threw: ${param.throwable}")
        // suppress the exception and return a default
        param.throwable = null
        param.result = fallbackValue
    }
}
```

## Performance

Each hooked method adds a small overhead per call -- the trampoline jump plus your callback code. For most use cases this is negligible. If you're hooking something that runs in a tight loop millions of times, you'll notice it.

JIT inlining is disabled globally when you call `init()`. This is necessary to make hooks reliable, but it means the JIT compiler can't inline any method calls across your app, not just hooked ones. In practice the performance difference is small, but it's worth knowing about.

## Cleanup

Call `Sandwitcher.reset()` to remove all hooks at once. Individual hooks can be removed with `handle.unhook()`.

There's no need to explicitly clean up on app exit -- the hooks live in-process and go away when the process dies.
