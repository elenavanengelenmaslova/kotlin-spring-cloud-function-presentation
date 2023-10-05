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

- ☁️ **Serverless Cloud**
- 🚀 **Function as a Service (FaaS)**
- 💡 **Kotlin on FaaS**
- 🧹 **Clean Architecture**
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

# Kotlin on FaaS: A Powerful Duo

<v-clicks>

- **Multi-Platform Compatibility**: Run on any FaaS supporting Java or JS, such as AWS Lambda and Azure Functions.

- **Bypass Java Version Constraints**: Utilize the newest features and advancements by employing the latest Kotlin versions, circumventing limitations imposed by cloud providers' supported Java versions (currently Java 17 on AWS and Azure).

- **Infrastructure as Code with Kotlin**: Employ Terraform CDK for multi-cloud setups, or AWS CDK, while keeping the concise, expressive, and safe syntax of Kotlin.

- **Unified with Kotlin**: Develop applications, manage infrastructure, and automate builds, all utilizing Kotlin's streamlined syntax and robust feature set.

</v-clicks>

<!--

Kotlin not only stands out due to its null safety and expressive syntax but also seamlessly works with serverless architectures. With its compatibility with various FaaS platforms and the ability to employ the latest Java versions, it provides a future-proof approach to crafting serverless applications. This is further bolstered by its capability to run natively and in JS environments, expanding use cases and ensuring your serverless applications can run anywhere and everywhere.
- Kotlin Function implementation
- Kotlin Infrastructure as code
- Kotlin Gradle DSL for builds

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

<img src="/CleanArch.png" alt="Clean Architecture" style="width: 80%; height: auto;" />


---

# Clean Architecture - with gradle modules

```
├── software/            // Holds all the application code
│   ├── domain/
│   ├── application/
│   └── infra/            // Infrastructure specific code
│       ├── aws/         
│       └── azure/       
│
└── cdk/                  // Terraform CDK Kotlin code
    ├── aws/
    └── azure/
```

<!--
Imagine we are build a pension administration microservice, 
- in domain we might have Participant, ParticipantRelation, PensionFund
- in application we have use case business logic, for example process employment, marriage, divorce, death
- infra has integration to our cloud specific service, e.g. Azure blob storage & Service Bus or AWS S3 and Event Bridge
- cdk has cloud specific infrastructure as code
-->

---

# Clean Architecture - with gradle modules
How would the module definition look in Gradle Kotlin DSL?

**settings.gradle.kts:**

```kotlin
include(":application")
project(":application").projectDir = file("software/application")
include(":domain")
project(":domain").projectDir = file("software/domain")
// ... other modules
```

**application/build.gradle.kts:**
```kotlin
dependencies {
   implementation(":domain")
}
```

---


# Spring Cloud Function in a Nutshell

<v-clicks>

- **Adaptable to Environments**: Facilitates execution in multiple environments - local, cloud, or FaaS, without code alterations.

- **Cloud Agnostic**: Enables applications to run across different FaaS providers like AWS Lambda, Azure Functions, etc.

- **Dependency Injection**: Harmonizes with Spring's robust dependency injection, allowing smooth integration with Clean Architecture.

- **Extensive Ecosystem**: Leverages the vast Spring ecosystem, unlocking a wide array of functionalities and extensibility for your serverless applications.

- **Memory and performance optimisation**: Utilize Spring Native with GraalVM to improve start-up performance and memory utilisation.

</v-clicks>


---

# Spring Cloud Function code example

```kotlin
@FunctionName("FileToEventProcessor")
@StorageAccount("AzureWebJobsStorage")
fun blobTrigger(
    @BlobTrigger(name = "content", path = "input/batch/someFile", dataType = "binary") 
    content: ByteArray,
    context: ExecutionContext,
) {
    handleContent(content)
}
```
---

# Terrform CDK

<v-clicks>

- **Multi language support**: Utilize familiar programming languages like Kotlin, Java or TypeScript for infrastructure code.

- **Multi-Cloud Compatibility**: Define and provision infrastructure seamlessly across multiple cloud providers like AWS, Azure, and Google Cloud.

- **Reusability**: Leverage constructs (modules) to create reusable, shareable, and composable components.

- **Interoperability**: Utilize existing Terraform providers and modules for an extensive library of resources.

- **Predictable Changes**: Employ `cdktf diff` and to understand the changes.

</v-clicks>

---

# Terrform CDK - Azure example

```kotlin
val functionApp = LinuxFunctionApp(
  this, "Terraform-Cdk-Kotlin-Azure-Function-JVM",
  LinuxFunctionAppConfig.builder()
    .name(functionAppName)
    // fun settings
    .siteConfig(
      LinuxFunctionAppSiteConfig.builder()
        .applicationStack(
          LinuxFunctionAppSiteConfigApplicationStack.builder()
            .javaVersion("17")
            .build()
        ).build()
    )
    .appSettings(
      // app settings
    )
    .build()
)
```

---

# Terrform CDK - AWS example

```kotlin
 LambdaFunction(
  this, "Terraform-Cdk-Kotlin-Lambda-JVM",
  LambdaFunctionConfig.builder()
    .functionName(functionName)
    .handler("nl.vintik.sample.KotlinLambda::handleRequest")
    .runtime("java17")
    .filename("${System.getenv("GITHUB_WORKSPACE")}/build/dist/function.zip")
    .architectures(listOf("arm64"))
    .role(lambdaRole.arn)
    .dependsOn(listOf(productsTable, lambdaRole))
    .memorySize(512)
    .timeout(120)
    .build()
)
```

---

# Deployment

```yaml {all|4-5|10-11}
- name: Generate Terraform files
  run: |
    cd ${GITHUB_WORKSPACE}/cdk
    cdktf get
    cdktf synth

- name: Deploy with Terraform
  run: |
    cd ${GITHUB_WORKSPACE}/cdk/cdktf.out/stacks/${{ matrix.config.stack-name }}
    terraform init
    terraform apply -auto-approve
```

<!--
Generate Terraform Files

- cdktf get: Fetches the dependencies required for the Terraform CDK code, such as provider plugins and modules.
- cdktf synth: Translates the CDKTF code into Terraform JSON configuration files, synthesizing the high-level constructs into a format Terraform understands.

Deploy with Terraform

- terraform init: Initializes the Terraform working directory, setting up the backend, and downloading the necessary provider plugins.
- terraform apply -auto-approve: Applies the changes necessary to reach the desired state of the configuration, automatically approving the applied plan, thus making changes to the infrastructure.

-->
---

# Performance Optimisations
Is performance an issue on FaaS? Short answer is - no.

If required you can configure performance optimisations such as AWS SnapStart (free) or Azure function Premium plan (costs).
E.g. in Terrform CDK:

```kotlin {all|8}
LambdaFunction(
  this, "Terraform-Cdk-Kotlin-Lambda-SnapStart",
  LambdaFunctionConfig.builder()
    .functionName(functionName)
    .handler("nl.vintik.sample.KotlinLambda::handleRequest")
    .runtime("java17")
    .filename("${System.getenv("GITHUB_WORKSPACE")}/build/dist/function.zip")
    .snapStart { "PublishedVersions" }
    .role(lambdaRole.arn)
    .memorySize(512)
    .timeout(120)
    .build()
)

```


---

# Key Takeaways
To conclude...

<v-clicks>

🌩️ **Achieving Cloud Independence**

- Clean Architecture: Minimizes cloud lock-in by keeping logic decoupled from infrastructure.
- Choose Wisely: Select tech that allows cloud-agnostic function implementations.

🛠️ **Kotlin: Everywhere**

- Uniformity: Use Kotlin across application development, infrastructure, and build automation.
- Compatibility: Kotlin enables usage across various FaaS providers and always stays up-to-date.

</v-clicks>


---
layout: end
---

# "Using FaaS for your cloud applications
## is like using a food delivery service for your meals."
> You get exactly what you want, when you want it, without dealing
> with the mess of cooking (or managing servers) yourself!
