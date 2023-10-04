---
# try also 'default' to start simple
theme: default
background: cover.jpeg
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: prism
canvasWidth: 800
# show line numbers in code blocks
lineNumbers: true

# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-up
# use UnoCSS
css: unocss
---

# Exploring Kotlin-Powered Serverless with Spring Cloud Function

<div class="pt-12">
    Elena van Engelen - Maslova
</div>


<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Overview

- 💡 **Introduction**
- ☁️ **Serverless Architecture**
- 🚀 **Function as a Service (FaaS)**
- 🧹 **Clean Architecture**
- 💻 **Kotlin on FaaS**
- 🌱 **Spring Cloud Function**
- 🌐 **Terraform CDK**
- 🛠️ **Deployment**
- 🗝️ **Key Takeaways**
- ❓ **Q&A**

<br>
<br>


<!--

-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->


---
transition: slide-up

level: 2
---

# What is Serverless cloud?

<v-clicks>

- **Resources on as-used basis**: Not waisting any resources by keeping resources running while waiting for requests.

- **No infrastructure management**: No patching and operating system upgrades.

- **Automatic Scaling**: Dynamically adjusts to the volume of transactions, scaling up or down automatically during peak and off-peak times.

- **Event-Driven**: Serverless seamlessly integrates with various event sources, offering a reactive paradigm that facilitates decoupling and promotes operational efficiency across diverse applications

</v-clicks>

<!--

- It is very resource-efficient as you pay only for the compute time you consume. This means no wastage of resources or energy since it scales down to zero when there's no traffic, ensuring you're not paying for idle compute resources
- Serverless architecture allows developers to build and run applications without having to manage the infrastructure. It abstracts and handles all the server management, allowing developers to focus solely on the code. There’s no need to worry about server maintenance, such as patching and operating system upgrades, thereby enabling a keener focus on developing functionalities.


-->

---

# What is Function-as-a-Service (FaaS)?

<v-clicks>

- **Microservices Paradigm**: Leverage smaller, self-contained functions to create microservices, enhancing modularity and maintainability.

- **Event Driven**: Functions act upon specific events or triggers, providing a dynamic and reactive interaction within cloud services.

- **Stateless Operations**: Functions are stateless, promoting scalability and ease of parallel execution without managing state persistence.

- **Resource Optimization**: Only consume resources while the function is executing, ensuring optimal usage and cost-effectiveness.

</v-clicks>

<!--
Examples of FaaS: AWS Lambda and Azure Function
-->

---
class: flex flex-col justify-center items-center h-[100vh] space-y-4
---

# FaaS on as-used basis

<img src="/FaaSAutoscaling.png" alt="FaaSAutoscaling" style="width: 60%; height: auto;" />

<!--
Example from online shop (bol.com) about season reparation for each and every application
-->

---

# FaaS Use-Cases

<v-clicks>

- **Microservice Architectures**: Small, independent functions communicate to form an application.

- **REST Backend**: Stateless, scalable API endpoints that are quick to build and easy to manage.

- **Stream Processing**: Analyze real-time data on the move, like from IoT devices or social media streams.

- **Data Processing**: Crunch numbers, process files, or validate data, all without maintaining a server.

</v-clicks>

<!--
FaaS provides an efficient and flexible way to create specific functionalities without the weight of managing the underlying infrastructure. The applications of serverless are diverse. From developing microservices, creating RESTful backends, managing stream processing, handling real-time file uploads, and conducting data processing, serverless provides a flexible and developer-friendly platform to build varied solutions
-->


---

# Dentist clinic use case

TODO
<img src="/FaaSAutoscaling.png" alt="FaaSAutoscaling" style="width: 60%; height: auto;" />

<!--
Example from online shop (bol.com) about season reparation for each and every application
-->

---

# Bridging to Clean Architecture

Scaling and flexibility are the hallmarks of FaaS, but how do we ensure our architecture remains cloud-agnostic and maintainable as it grows?

<v-clicks>

- **Separation of Concerns**: Different aspects of software development (use case business rules, domain logic, integration logic) are isolated from each other.

- **Independent Layers**: Changes in one layer (like switching cloud providers) should not affect other layers.

- **Testability**: Because of the clear boundaries and interfaces between layers, testing becomes straightforward.

- **Minimizing Cloud Lock-in**: Easily switch between AWS, Azure, or others, with business logic unaware of the underlying cloud provider.

</v-clicks>

<!--

Clean Architecture allows us to create a system that is:
- Independent of the UI and other application api entry points like Rest and GraphQL
- Independent of the database providers
- Independent of any external providers
- Testable

In the context of FaaS and serverless, it provides a pathway to ensure that our functions are not tightly bound to a specific cloud provider's APIs or services, ensuring that our application logic remains versatile, testable, and scalable, while also being easy to migrate between different platforms.

-->

---
class: flex flex-col justify-center items-center h-[100vh] space-y-4
---

# Clean Architecture

<img src="/FaaSAutoscaling.png" alt="Clean Architecture" style="width: 60%; height: auto;" />


---

# Clean Architecture - example with gradle modules


---

# Kotlin on FaaS: A Powerful Duo


<v-clicks>

- **Multi-Platform Compatibility**: Run on any FaaS supporting Java or JS, such as AWS Lambda and Azure Functions.

- **Bypass Java Version Constraints**: Utilize the newest features and advancements by employing the latest Kotlin versions, circumventing limitations imposed by cloud providers' supported Java versions (currently Java 17 on AWS and Azure).

- **Infrastructure as Code with Kotlin**: Employ Terraform CDK for multi-cloud setups, or AWS CDK, while keeping the concise, expressive, and safe syntax of Kotlin.

- **Unified with Kotlin**: Develop applications, manage infrastructure, and automate builds, all utilizing Kotlin's streamlined syntax and robust feature set.

</v-clicks>

<!--

Kotlin not only stands out due to its null safety and expressive syntax but also seamlessly works with serverless architectures. With its compatibility with various FaaS platforms and the ability to employ the latest Java versions, it provides a future-proof approach to crafting serverless applications. This is further bolstered by its capability to run natively and in JS environments, expanding use cases and ensuring your serverless applications can run anywhere and everywhere.
- Kotlin app
- Kotlin Infra
- Kotlin Gradle DSL

-->

---

# Spring Cloud Function in a Nutshell

<v-clicks>

- **Adaptable to Environments**: Facilitates execution in multiple environments - local, cloud, or FaaS, without code alterations.

- **Cloud Agnosticism**: Enables applications to run across different FaaS providers like AWS Lambda, Azure Functions, etc.

- **Dependency Injection**: Harmonizes with Spring's robust dependency injection, allowing smooth integration with Clean Architecture.

- **Extensive Ecosystem**: Leverages the vast Spring ecosystem, unlocking a wide array of functionalities and extensibility for your serverless applications.

- **Unified API**: Offers a consistent programming model across serverless providers.

</v-clicks>

---

# Spring Cloud Function code example


---


---
layout: end
---

# "Using FaaS for your cloud applications
## is like using a food delivery service for your meals."
> You get exactly what you want, when you want it, without dealing
> with the mess of cooking (or managing servers) yourself!
