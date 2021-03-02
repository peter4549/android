# [Understand How does Retrofit work](https://medium.com/mindorks/understand-how-does-retrofit-work-c9e264131f4a)
<p align="center">
<img alt="Image for post" class="vf wr t u v hr aj c" width="931" height="415" src="https://miro.medium.com/max/931/1*LSeA2e6nf6FezMDtHKmO7g.png" srcset="https://miro.medium.com/max/276/1*LSeA2e6nf6FezMDtHKmO7g.png 276w, https://miro.medium.com/max/552/1*LSeA2e6nf6FezMDtHKmO7g.png 552w, https://miro.medium.com/max/640/1*LSeA2e6nf6FezMDtHKmO7g.png 640w, https://miro.medium.com/max/700/1*LSeA2e6nf6FezMDtHKmO7g.png 700w" sizes="700px">
</p>

### What
**Retrofit** is a type-safe HTTP client for Android and Java.

### Why
Using Retrofit made networking easier in Android apps. As it has many features like easy to add custom headers and request types, file uploads, mocking responses, etc through which we can reduce boilerplate code in our apps and consume the web service easily.

### How
To work with Retrofit we basically need the following three classes:
* A model class which is used as a **JSON** model
* An interface that defines the **HTTP** operations needs to be performed
* Retrofit.Builder class: Instance which uses the interface defined above and the Builder API to allow defining the **URL endpoint** for the **HTTP** operations. It also takes the **converters** we provide to format the **Response**.

## Behind the scenes
```
val shareService = retrofit.create(ApiService.class);
```
Click on the create method which will navigate you to **Retrofit.java** class to the **create method** which looks like below
```
  public <T> T create(final Class<T> service) {
    Utils.validateServiceInterface(service);
    if (validateEagerly) {
      eagerlyValidateMethods(service);
    }
    return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
        new InvocationHandler() {
          private final Platform platform = Platform.get();
          private final Object[] emptyArgs = new Object[0];

          @Override public @Nullable Object invoke(Object proxy, Method method,
              @Nullable Object[] args) throws Throwable {
            // If the method is a method from Object then defer to normal invocation.
            if (method.getDeclaringClass() == Object.class) {
              return method.invoke(this, args);
            }
            if (platform.isDefaultMethod(method)) {
              return platform.invokeDefaultMethod(method, service, proxy, args);
            }
            return loadServiceMethod(method).invoke(args != null ? args : emptyArgs);
          }
        });
  }
```
The first check it does is whether the interface provided is a **valid** interface or not by calling the method **validateServiceInterface** If it was not an interface it throws **IllegalArgumentException**
```
  static <T> void validateServiceInterface(Class<T> service) {
    if (!service.isInterface()) {
      throw new IllegalArgumentException("API declarations must be interfaces.");
    }
    // Prevent API interfaces from extending other interfaces. This not only avoids a bug in
    // Android (http://b.android.com/58753) but it forces composition of API declarations which is
    // the recommended pattern.
    if (service.getInterfaces().length > 0) {
      throw new IllegalArgumentException("API interfaces must not extend other interfaces.");
    }
  }
```
The Second Step is like a chain of methods starting from eagerlyValidateMethods method call.
```
  private void eagerlyValidateMethods(Class<?> service) {
    Platform platform = Platform.get();
    for (Method method : service.getDeclaredMethods()) {
      if (!platform.isDefaultMethod(method) && !Modifier.isStatic(method.getModifiers())) {
        loadServiceMethod(method);
      }
    }
  }
```
In the above method **Platform.get()** has the following implementation to return the **platform type**.
```
class Platform {
  private static final Platform PLATFORM = findPlatform();

  static Platform get() {
    return PLATFORM;
  }

  private static Platform findPlatform() {
    try {
      Class.forName("android.os.Build");
      if (Build.VERSION.SDK_INT != 0) {
        return new Android();
      }
    } catch (ClassNotFoundException ignored) {
    }
    try {
      Class.forName("java.util.Optional");
      return new Java8();
    } catch (ClassNotFoundException ignored) {
    }
    return new Platform();
  }

  @Nullable Executor defaultCallbackExecutor() {
    return null;
  }

  List<? extends CallAdapter.Factory> defaultCallAdapterFactories(
      @Nullable Executor callbackExecutor) {
    return singletonList(new DefaultCallAdapterFactory(callbackExecutor));
  }

  int defaultCallAdapterFactoriesSize() {
    return 1;
  }

  List<? extends Converter.Factory> defaultConverterFactories() {
    return emptyList();
  }

  int defaultConverterFactoriesSize() {
    return 0;
  }

  boolean isDefaultMethod(Method method) {
    return false;
  }

  @Nullable Object invokeDefaultMethod(Method method, Class<?> declaringClass, Object object,
      @Nullable Object... args) throws Throwable {
    throw new UnsupportedOperationException();
  }

  @IgnoreJRERequirement // Only classloaded and used on Java 8.
  static class Java8 extends Platform {
    @Override boolean isDefaultMethod(Method method) {
      return method.isDefault();
    }

    @Override Object invokeDefaultMethod(Method method, Class<?> declaringClass, Object object,
        @Nullable Object... args) throws Throwable {
      // Because the service interface might not be public, we need to use a MethodHandle lookup
      // that ignores the visibility of the declaringClass.
      Constructor<Lookup> constructor = Lookup.class.getDeclaredConstructor(Class.class, int.class);
      constructor.setAccessible(true);
      return constructor.newInstance(declaringClass, -1 /* trusted */)
          .unreflectSpecial(method, declaringClass)
          .bindTo(object)
          .invokeWithArguments(args);
    }

    @Override List<? extends CallAdapter.Factory> defaultCallAdapterFactories(
        @Nullable Executor callbackExecutor) {
      List<CallAdapter.Factory> factories = new ArrayList<>(2);
      factories.add(CompletableFutureCallAdapterFactory.INSTANCE);
      factories.add(new DefaultCallAdapterFactory(callbackExecutor));
      return unmodifiableList(factories);
    }

    @Override int defaultCallAdapterFactoriesSize() {
      return 2;
    }

    @Override List<? extends Converter.Factory> defaultConverterFactories() {
      return singletonList(OptionalConverterFactory.INSTANCE);
    }

    @Override int defaultConverterFactoriesSize() {
      return 1;
    }
  }

  static class Android extends Platform {
    @IgnoreJRERequirement // Guarded by API check.
    @Override boolean isDefaultMethod(Method method) {
      if (Build.VERSION.SDK_INT < 24) {
        return false;
      }
      return method.isDefault();
    }

    @Override public Executor defaultCallbackExecutor() {
      return new MainThreadExecutor();
    }

    @Override List<? extends CallAdapter.Factory> defaultCallAdapterFactories(
        @Nullable Executor callbackExecutor) {
      if (callbackExecutor == null) throw new AssertionError();
      DefaultCallAdapterFactory executorFactory = new DefaultCallAdapterFactory(callbackExecutor);
      return Build.VERSION.SDK_INT >= 24
        ? asList(CompletableFutureCallAdapterFactory.INSTANCE, executorFactory)
        : singletonList(executorFactory);
    }

    @Override int defaultCallAdapterFactoriesSize() {
      return Build.VERSION.SDK_INT >= 24 ? 2 : 1;
    }

    @Override List<? extends Converter.Factory> defaultConverterFactories() {
      return Build.VERSION.SDK_INT >= 24
          ? singletonList(OptionalConverterFactory.INSTANCE)
          : Collections.<Converter.Factory>emptyList();
    }

    @Override int defaultConverterFactoriesSize() {
      return Build.VERSION.SDK_INT >= 24 ? 1 : 0;
    }

    static class MainThreadExecutor implements Executor {
      private final Handler handler = new Handler(Looper.getMainLooper());

      @Override public void execute(Runnable r) {
        handler.post(r);
      }
    }
  }
}
```
After getting platform type it calls **service.getDeclaredMethods()** inside **eagerlyValidateMethods()** which returns an array containing Method objects reflecting all the declared methods of the class or interface represented by this object, including public, protected, default (package)access, and private methods, but excluding inherited methods.
```
public Method[] getDeclaredMethods() throws SecurityException {
    Method[] result = getDeclaredMethodsUnchecked(false);
    for (Method m : result) {
        // Throw NoClassDefFoundError if types cannot be resolved.
        m.getReturnType();
        m.getParameterTypes();
    }
    return result;
}
```
There is an iteration over the methods array obtained to identify the **request type, method name, Annotations** and **arguments** which will be stored in a **serviceMethodCache** map as shown below
```
private final Map<Method, ServiceMethod<?>> serviceMethodCache = new ConcurrentHashMap<>();
ServiceMethod<?> loadServiceMethod(Method method) {
  ServiceMethod<?> result = serviceMethodCache.get(method);
  if (result != null) return result;

  synchronized (serviceMethodCache) {
    result = serviceMethodCache.get(method);
    if (result == null) {
      result = ServiceMethod.parseAnnotations(this, method);
      serviceMethodCache.put(method, result);
    }
  }
  return result;
}
```
**ServiceMethod** is an abstract class that has the following details
```
package retrofit2;

import java.lang.reflect.Method;
import java.lang.reflect.Type;
import javax.annotation.Nullable;

import static retrofit2.Utils.methodError;

abstract class ServiceMethod<T> {
  static <T> ServiceMethod<T> parseAnnotations(Retrofit retrofit, Method method) {
    RequestFactory requestFactory = RequestFactory.parseAnnotations(retrofit, method);

    Type returnType = method.getGenericReturnType();
    if (Utils.hasUnresolvableType(returnType)) {
      throw methodError(method,
          "Method return type must not include a type variable or wildcard: %s", returnType);
    }
    if (returnType == void.class) {
      throw methodError(method, "Service methods cannot return void.");
    }

    return HttpServiceMethod.parseAnnotations(retrofit, method, requestFactory);
  }

  abstract @Nullable T invoke(Object[] args);
}
```
This in turn again calls the final class **RequestFactory** in which the **Builder** method has the following code.
```
Builder(Retrofit retrofit, Method method) {
  this.retrofit = retrofit;
  this.method = method;
  this.methodAnnotations = method.getAnnotations();
  this.parameterTypes = method.getGenericParameterTypes();
  this.parameterAnnotationsArray = method.getParameterAnnotations();
}
```
<p align="center">
<img alt="Image for post" class="vf ws t u v hr aj c" src="https://miro.medium.com/max/2143/1*4icKveo2LcB84EgBOpD3-g.png" >
</p>  

Finally, we reach **java.lang.reflect** package to fetch **annotations** from **AccessibleObject** class, **getGenericParameterTypes()** and **getParameterAnnotations()** from **Method** class.

## Reflections
