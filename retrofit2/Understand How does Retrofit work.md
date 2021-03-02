# [Understand How does Retrofit work](https://medium.com/mindorks/understand-how-does-retrofit-work-c9e264131f4a)
<img alt="Image for post" class="vf wr t u v hr aj c" width="931" height="415" src="https://miro.medium.com/max/931/1*LSeA2e6nf6FezMDtHKmO7g.png" srcset="https://miro.medium.com/max/276/1*LSeA2e6nf6FezMDtHKmO7g.png 276w, https://miro.medium.com/max/552/1*LSeA2e6nf6FezMDtHKmO7g.png 552w, https://miro.medium.com/max/640/1*LSeA2e6nf6FezMDtHKmO7g.png 640w, https://miro.medium.com/max/700/1*LSeA2e6nf6FezMDtHKmO7g.png 700w" sizes="700px">

### What
**Retrofit** is a type-safe HTTP client for Android and Java.

### Why
Using Retrofit made networking easier in Android apps. As it has many features like easy to add custom headers and request types, file uploads, mocking responses, etc through which we can reduce boilerplate code in our apps and consume the web service easily.

### How
To work with Retrofit we basically need the following three classes:
* A model class which is used as a **JSON** model
* An interface that defines the **HTTP** operations needs to be performed
* Retrofit.Builder class: Instance which uses the interface defined above and the Builder API to allow defining the **URL endpoint** for the **HTTP** operations. It also takes the **converters** we provide to format the **Response**.
