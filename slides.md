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

# Clean Architecture for Serverless: Business Logic You Can Take Anywhere

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

- ☁️🚀 **Serverless & FaaS**
- 💡 **Kotlin on FaaS**
- 🧹 **Clean Architecture for Serverless**
- 🧑‍💻 **Live Coding & Demo – Part 1**
- 🧱 **Gradle Modules**
- 🌱 **Spring Cloud Function**
- 🧑‍💻 **Live Coding & Demo – Part 2**
- 🌐 **Terraform CDK**
- 🛠️ **Deployment**
- 🗝️ **Key Takeaways**
- ❓ **Q&A**


<br>
<br>




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
- Lets look at the talk structure
- First I will introduce serverless and Function as a Service, why we should use it and when is it usefull
- We will look at why we chose Kotlin for serverless
- Then I will discuss clean architecture for serverless
- With live coding we will jump streight to craete a real world application, lets not do hello world anyone can do that. First part is where we actually replace hello world by wiring in and deploying same business logic to AWS Lambda and Azure function.
- while we are deploying will will zoom into how we implemented gradle modules and talk about spring cloud function
- We then do a quick demo of the results before starting net part of live coding
- second part will involve actually plugging in cloud specific persistence to our portalble business logic without adding any cloud dependencies into the business logic code
- again deploy tha and while deploying we will look at Terraform CDK in Kotlin
- After deploy, we will demo the final results
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
class: flex flex-col justify-center items-center h-[100vh] space-y-4
---

# FaaS on as-used basis

<img src="/FaaSAutoscaling.png" alt="FaaSAutoscaling" style="width: 60%; height: auto;" />

<!--
Example from online shop (bol.com) about season reparation for each and every application
-->

---
class: flex flex-col justify-center items-center h-[100vh] text-center space-y-4 px-8
---

### <span class="text-4xl font-bold">Using FaaS for your cloud applications</span>

### <span class="text-2xl">is like using a food delivery service for your meals.</span>

<span class="text-lg italic text-slate-600">
You get exactly what you want, when you want it —  
without dealing with the mess of cooking (or managing servers) yourself!
</span>

---

# Kotlin on FaaS: A Powerful Duo

<v-clicks>

- **Multi-Platform Compatibility**: Run on any FaaS supporting Java or JS, such as AWS Lambda and Azure Functions.

- **Bypass Java Version Constraints**: Utilize the newest features and advancements by employing the latest Kotlin versions, circumventing limitations imposed by cloud providers' supported Java versions (currently Java 21 on AWS and Java 17 on Azure).

- **Infrastructure as Code with Kotlin**: Use Terraform CDK for multi-cloud setups, while keeping the concise, expressive, and safe syntax of Kotlin.

- **Unified with Kotlin**: Use Gradle with kotlin DSL. Develop applications, manage infrastructure, and automate builds, all utilizing Kotlin's streamlined syntax and robust feature set.

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
class: flex flex-col justify-start items-center h-[100vh] space-y-6 px-8
---

# Too Complex for FaaS?

<div class="grid grid-cols-3 gap-4 w-full justify-items-center">
  <div class="flex flex-col items-center space-y-2">
    <img src="/Onion.png" alt="Onion Architecture" class="h-[35vh] object-contain" />
    <span class="text-center text-base text-slate-600">Onion Architecture</span>
  </div>

  <div class="flex flex-col items-center space-y-2">
    <img src="/Hexagonal.png" alt="Hexagonal Architecture" class="h-[35vh] object-contain" />
    <span class="text-center text-base text-slate-600">Hexagonal Architecture</span>
  </div>

  <div class="flex flex-col items-center space-y-2">
    <img src="/FullClean.jpeg" alt="Clean Architecture" class="h-[35vh] object-contain" />
    <span class="text-center text-base text-slate-600">Clean Architecture</span>
  </div>
</div>

---
class: flex flex-col justify-center items-center h-[100vh] space-y-4
---

# Clean Architecture for Serverless

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


MockNest: 
- in domain we would have everything with mocking, in our case wiremock has all the logic so we do not have much here
- in application we have use case business logic, so actual forwarding logic to wiremock, and also any extra functionality specific for the use cases rather than domain
- infra has integration to our cloud specific service, e.g. Azure blob storage and Azure function
AWS S3 and AWS lambda
- cdk has cloud specific infrastructure as code
-->

---

# Use Case - MockNest
_Serverless WireMock_

<br>

<img src="UseCases.png" alt="Use Cases" class="max-w-[90%] max-h-[80vh] object-contain mx-auto" />

<!--
(Call AWS)

- We are going to basically run WireMock from our serverless FaaS with some persistence. Wiremock is a library that allows you to mock external APIs. How many of you used WireMock?
- MockNest is usefull for external systems which are not accessible in your particular cloud environment, e.g. development, or where test data and edge cases are difficult to set up. e.g. time travel

- Mock any external REST or SOAP service

- Ability switch between mock and real service

- Ability to run automated integration tests

- Ability to test manually
- MockNest should give you the ability to mock and REST or SOAP API by wiring requests to responses, this way you can test all sorts of situations including edge cases and error flows
- You can switch between the real external service or the mock end point by just pointing at the appropriate URL
- You can then set up your test scenarios for automated or manual tests, not for manual tests our mock data needs t remain persistent as out mock may scale down to 0 when no one is testing.
- It should be persistent because we are on serverless


-->

---

## Solution Design
<br>

<img src="AzureMockNest.png" alt="Solution Design" class="max-w-[60%] max-h-[60vh] object-contain mx-auto" />

<!-- 
(Call Azure)

So wer are not using a Hello world, however the use case is still simple, we hve some business logic which requires some persistence, and we are using a cloud specific service for this persistence. In AWS we will use S3, and in Azure we are using Blob Storage. These are exactly what we need to store our mock configuration.
-->

---

# 🧑‍💻 Live Coding – Part 1: From Hello World to Business Logic

<v-clicks>

- Walk through project structure & setup  
- Connect business logic to AWS Lambda & Azure Function  
- Deploy and Demo "MockNest" on both AWS and Azure

</v-clicks>

<!--
Let's update our Hello world Lambda and Azure function to use the busness logic which is a WireMock with some forwarding logic for serverless:
- update module dependencies
```kotlin
implementation(project(":domain"))
```
- wire in business logic from functions

```kotlin
private val handleWireMockRequest: HandleWireMockRequest,
private val handleAdminRequest: HandleAdminRequest,
```

- Implement Azure wiremock request handling

```kotlin
val response = handleWireMockRequest(
            HttpRequest(
                org.springframework.http.HttpMethod.valueOf(request.httpMethod.name),
                request.headers,
                route,
                request.queryParameters,
                request.body
            )
        )
```

- Implement Azure wiremock response handling

```kotlin
val response = handleAdminRequest(
            route ?: "",
            HttpRequest(
                SpringHttpMethod.valueOf(request.httpMethod.name),
                request.headers,
                route,
                request.queryParameters,
                request.body
            )
        )
```

- map wiremock response to azure api response

```kotlin
.createResponseBuilder(HttpStatus.valueOf(response.httpStatusCode.value()))
            .let { responseBuilder ->
                var builder = responseBuilder
                response.headers?.forEach { header ->
                    header.value.forEach {
                        builder = builder.header(header.key, it)
                    }
                }
                builder
            }
            .body(response.body)

```
- Add dependency and inject our request handling functions into AWS Lambda
- Convert our Lambda secific request to oour domain object

```kotlin
 private fun APIGatewayProxyRequestEvent.createHttpRequest(path: String): HttpRequest {
        val request = HttpRequest(
            method = HttpMethod.valueOf(httpMethod),
            headers = headers,
            path = path,
            queryParameters = queryStringParameters.orEmpty(),
            body = body
        )
        return request
    }
```

- call injected function and convert response

```kotlin
  with(event) {
                logger.info { "MockNest request: $httpMethod $path $headers" }
                if (path.startsWith(ADMIN_PREFIX)) {
                    val adminPath = path.removePrefix(ADMIN_PREFIX)
                    handleAdminRequest(adminPath, createHttpRequest(adminPath))
                } else {
                    handleWireMockRequest(createHttpRequest(path.removePrefix(WIREMOCK_PREFIX)))
                }
            }.let {
                APIGatewayProxyResponseEvent()
                    .withStatusCode(it.httpStatusCode.value())
                    .withHeaders(it.headers?.toSingleValueMap())
                    .withBody(it.body?.toString().orEmpty())
            }
```

- build, commit and push


-->

---

# Clean Architecture - With Gradle Modules
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
  implementation(project(":domain"))
}
```

---

# Spring Cloud Function in a Nutshell
Enables you to run your function implementation as a Spring app

<v-clicks>

- **Cloud Agnostic**: Enables applications to run across different FaaS providers like AWS Lambda, Azure Functions, etc.

- **Adaptable to Environments**: Facilitates execution in multiple environments - local or cloud

- **Dependency Injection**: Harmonizes with Spring's  dependency injection, allowing smooth integration with Clean Architecture.

- **Memory and performance optimisation**: Utilize Spring Native with GraalVM to improve start-up performance and memory utilisation.

</v-clicks>

<!--
- Spring cloud function essentially lets you run a spring app within your FaaS. 
In that it is cloud agnostic. However just like terraform it requires specific adapters for each cloud providers.
- Whereby entry point of the function has cloud specific configuration. So you speak the same language but you use dufferent dialecvt to describe your function triggers. Nice try spring with generic object model..
- Important for us specifically for our clean architecture and separation of cloud specific code is the dependency injection. Where we inject implementations, such as integration with a cloud specific database, storage service, or messaging service into the business logic wihout coupling business logc to specific cloud provider.
- You can use Spring's extensive ecosystem. 
- You can even use GraalVM for performance optimisation, however in my experience this complexity is not needed because of moddern solutions like Lambda SnapStart (free for JVM) or Azure Function Elastic Premium plan (not free)

-->

---

# AWS Function Code Examples
Insure to include spring cloud function adapter in infrastructure layer dependencies.

### build.gradle.kts in AWS infrastructure module:

```kotlin
implementation("org.springframework.cloud:spring-cloud-function-adapter-aws:4.2.2")
```

### AWS Lambda

```kotlin
@Bean
fun router(): Function<APIGatewayProxyRequestEvent, APIGatewayProxyResponseEvent> {
    return Function { event ->
        APIGatewayProxyResponseEvent()
            .withStatusCode(200)
            .withBody("Hello VoxxedDays Amsterdam 2025!")
    }
}
```

---

# Azure Function Code Examples
Insure to include spring cloud function adapter in infrastructure layer dependencies.

### build.gradle.kts in Azure infrastructure module:

```kotlin
implementation("org.springframework.cloud:spring-cloud-function-adapter-azure:4.2.2")
```

### Azure function

```kotlin
@FunctionName("WiremockForwarder")
    fun forwardToWiremock(
        @HttpTrigger(
            methods = [HttpMethod.POST, HttpMethod.GET, HttpMethod.PATCH, HttpMethod.PUT, HttpMethod.DELETE],
            authLevel = AuthorizationLevel.FUNCTION,
            name = "request", route = "wiremock/{*route}"
        ) request: HttpRequestMessage<String>, context: ExecutionContext,
    ): HttpResponseMessage {
        return buildResponse(request)
    }
```

---

# Demo 1

---


# 🧑‍💻 Live Coding – Part 2: Add Persistence

<v-clicks>

- Walk though persistence code for storing mock mappings
- Extend business logic persistence integration
- Deploy and Demo the updated "MockNest" on both AWS and Azure

</v-clicks>

<!--
Let's update our Hello world Lambda and Azure function to use the busness logic which is a WireMock with some forwarding logic for serverless: 
- inject repository
```kotlin
private val wireMockMappingRepository: WireMockMappingRepository,
```

- call business logic from functions

- add stub mapping
```kotlin
 private fun saveStubMapping(mapping: StubMapping, bodyString: String, ): String {
        return mapping.runCatching {
            if (isPersistent) {
                logger.info { "Saving persistent mapping with ID: $id" }
                wireMockMappingRepository.saveMapping(id.toString(), bodyString).let { "Saved mapping $it" }
            } else "Mapping ${mapping.id} not persistent"
        }.onFailure {
            logger.error(it) { "Failed to check or save persistent mapping: ${it.message}" }
        }.getOrThrow()
    }

 saveStubMapping(mapping, bodyString)
```

- delete all mappings when we call reset
```kotlin
 // Delete all mappings from storage
                    val storedMappings = wireMockMappingRepository.listMappings()
                    storedMappings.forEach { mappingId ->
                        logger.info { "Deleting stored WireMock mapping with ID: $mappingId" }
                        wireMockMappingRepository.deleteMapping(mappingId)
                    }
```



-->

---

# Terraform CDK

<v-clicks>

- **Multi language support**: Utilize familiar programming languages like Kotlin, Java or TypeScript for infrastructure code.

- **Multi-Cloud Compatibility**: Define and provision infrastructure seamlessly across multiple cloud providers like AWS, Azure, and Google Cloud.

- **Reusability**: Leverage constructs (modules) to create reusable, shareable, and composable components.

- **Interoperability**: Utilize existing Terraform providers and modules for an extensive library of resources.

- **Predictable Changes**: Employ `cdktf diff` and to understand the changes.

</v-clicks>

<!--
Generates terraform instead of cloud formation. 
- Its like speaking the same language but having to use diffferent words to describe your infrastructure. It's like a natural language - much easier to learn new workds of a language you already know than to learn a new language from scratch.
-->
---


# Terrform CDK - Azure example


```kotlin

val functionApp = LinuxFunctionApp(
  this, "SpringCloudExampleFunctionApp",
  LinuxFunctionAppConfig.builder()
    .name("spring-clean-architecture-fun")
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

# Terraform CDK - AWS example

```kotlin {all|5,14,15}
 LambdaFunction(
  this, "Spring-Clean-Architecture-Fun",
  LambdaFunctionConfig.builder()
    .functionName(functionName)
    .handler("org.springframework.cloud.function.adapter.aws.FunctionInvoker")
    .runtime("java17")
    // other settings
    .role(lambdaRole.arn)
    .dependsOn(listOf(productsTable, lambdaRole))
    .environment(
      LambdaFunctionEnvironment.builder()
        .variables(
          mapOf(
            "SPRING_CLOUD_FUNCTION_DEFINITION" to "router",
            "MAIN_CLASS" to "com.example.clean.architecture.Application",
        ).build()
    ).build()
)
```

---

# Deployment

```yaml {all|4-5|10-12}
- name: Generate Terraform files
  run: |
    cd ${GITHUB_WORKSPACE}/cdk
    cdktf get
    cdktf synth

- name: Deploy with Terraform
  run: |
    cd ${GITHUB_WORKSPACE}/cdk/cdktf.out/stacks/${{ matrix.config.stack-name }}
    terraform init -reconfigure
    terraform plan -out=tfplan
    terraform apply -auto-approve tfplan
```

<!--
(Run AWS)
Generate Terraform Files

- cdktf get: Fetches the dependencies required for the Terraform CDK code, such as necessary Terraform providers and modules referenced in your CDKTF code.
- cdktf synth: Converts your Kotlin CDKTF code into Terraform JSON files.

Deploy with Terraform

- terraform init -reconfigure

Initializes the Terraform working directory (downloads providers, configures backends).
-reconfigure forces Terraform to ignore any previously saved backend config and re-read it from main.tf.json.
If you're using a remote backend like S3 (AWS), this ensures Terraform reads the real current state from there, instead of looking at a local .tfstate file.
- terraform plan -out=tfplan

Creates an execution plan by comparing the desired state (your Terraform files) with the current state (from the backend).
The plan is saved to a file called tfplan.
This file will then be used for the actual apply step, so you're guaranteed to apply exactly what was planned.

- terraform apply -auto-approve tfplan

Applies the previously generated plan (tfplan) without prompting for approval. What could possibly go wrong? ;)
This ensures only the changes you already reviewed or tested in the plan step are applied — no surprises.
-->

---
class: flex flex-col justify-center items-center h-[100vh] text-center space-y-4 px-8
---

### <span class="text-4xl font-bold">Terraform</span>

### <span class="text-2xl">lets you speak the same language — just in a different dialect.</span>

<span class="text-lg italic text-slate-600">
Switching clouds doesn’t mean learning everything from scratch —  
you already speak the language. It’s just a matter of picking up a few new words to match the local dialect.
</span>

<!--
(Run Azure)
Open build and Check with audience deploymemt status
-->

---

# Demo 2

---

# Key Takeaways
To conclude...

<v-clicks>

🧹 **Keep Business Logic Clean and Portable**  
Use Clean Architecture and Spring Cloud Function to decouple core logic from cloud-specific code.

📦 **Structure with Gradle Modules**  
Enforce clear boundaries between business, application, and infrastructure layers using modular Gradle projects.

🌍 **Deploy and Run Anywhere**  
Once isolated, your business logic can run on various cloud providers, e.g.AWS and Azure

🛠️ **Kotlin Everywhere**
Speak the same lingo - across application development, infrastructure, and build automation.

</v-clicks>

<!-- 
 - Clean architecture enables you to keep your business logic separate from cloud specific code and therefore portable across clouds
 - Speak the same language - it helps portability to keep the language and tooling the same thus also not cloud dependent
-->

---
class: flex flex-col items-center justify-start h-[100vh] pt-8 space-y-4 px-8
---

# ❓ Q&A

<div class="text-base text-slate-500 text-center mt-2">
Feel free to ask anything — architecture, Kotlin, or serverless!
</div>

<div class="flex flex-row justify-center gap-20 pt-6">

  <!-- Left: Connect with me -->
  <div class="flex flex-col items-center space-y-2 text-sm text-slate-500">
    <img src="/website-qr.png" alt="QR code to personal website" class="w-28 rounded shadow" />
    <div><strong>Connect with me</strong> 🌐</div>
    <a href="https://elenavanengelenmaslova.github.io/" target="_blank" class="text-blue-600 underline">
      elenavanengelenmaslova.github.io
    </a>
  </div>

  <!-- Right: Book -->
  <div class="flex flex-col items-center space-y-2 text-sm text-slate-500">
    <img src="/book-qr.png" alt="QR code to Kotlin Crash Course" class="w-28 rounded shadow" />
    <div><strong>Kotlin Crash Course</strong> 📘</div>
    <a href="https://qrco.de/bfquzM" target="_blank" class="text-blue-600 underline">
      qrco.de/bfquzM
    </a>
    <div>Use code <span class="font-semibold text-slate-700">VOXXEDAMS15</span> for 15% off</div>
    <div class="italic text-xs text-slate-400">Valid until: May 31</div>
  </div>

</div>



<!-- 
 If you thought this is cool but i first need to brush up on my Kotlin before deploying to cloud, then I recommend my book. at the end, lucky chapter 13 guides you through deploying an event driven serverless app
-->


