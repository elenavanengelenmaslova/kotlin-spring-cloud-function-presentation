---
theme: default
background: cover.jpeg
class: text-center
highlighter: prism
canvasWidth: 800
lineNumbers: true
drawings:
  persist: false
transition: slide-up
css: unocss
title: 'Clean Architecture for Serverless: Business Logic You Can Take Anywhere'
---

# Clean Architecture for Serverless: Business Logic You Can Take Anywhere

<div class="pt-12">
    Elena van Engelen - Maslova
</div>

---
class: px-8
---

# Hi, I'm Elena van Engelen 👋

<v-clicks>

<!-- Bullet points -->
<ul class="text-left text-lg leading-relaxed text-slate-700 space-y-4 max-w-3xl">
  <li><strong>Senior Software Engineer</strong> specializing in Kotlin and cloud-native solutions.</li>
  <li>4× AWS and 2× Azure certified</li>
  <li>Currently working as <strong>Lead Engineer</strong> at <strong>AZL (Life & Pensions – NN Group)</strong>.</li>
  <li><strong>Author</strong> of <em>Kotlin Crash Course</em> and a <strong>blogger</strong> on Kotlin & Cloud topics.</li>
</ul>

<!-- Final click: image + quote -->
<div class="flex flex-col md:flex-row gap-6 items-center pt-8 max-w-4xl">

  <!-- Image: reduced size -->
  <div class="flex-shrink-0">
    <img src="/familyCarnaval.jpeg" alt="My family at Dutch Carnaval"
      class="rounded-xl shadow-lg w-[200px] h-auto object-cover" />
  </div>

  <!-- Quote -->
  <div class="italic text-slate-600 text-lg leading-relaxed text-center md:text-left px-2">
    "I explain why Kotlin and Cloud is so much fun.<br />
    They explain why Carnaval is so much fun.<br />
    We’ve reached perfect mutual misunderstanding."
  </div>

</div>

</v-clicks>

---
transition: fade-out
---

# Overview

<v-clicks>

- 🌱 **Spring Cloud Function**

- 🧼 **Clean Architecture**

- 🌐 **Deployment & Terraform CDK**

- 🗝️ **Key Takeaways + Q&A**
  
</v-clicks>

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
-- TODO
-->

---
transition: slide-up
layout: image-right
image: /Scaling.png
---

# What is Serverless and FaaS?

<v-clicks>

- Resources on As-Used Basis

- No Infrastructure to Manage

- Scales Automatically

- Triggered by Events

</v-clicks>

<!--
Serverless doesn’t mean there are no servers — it just means you don’t have to manage them.

Resources on as-used basis:
Serverless is very resource-efficient — you only pay for the compute time you actually consume. It scales to zero when there's no traffic, so you're not paying for idle capacity.

No infrastructure to manage:
You don’t need to manage or patch servers. That’s handled by the cloud provider. This frees you up to focus on writing business logic instead of operations.

Scales automatically:
Serverless functions adjust automatically to load — they scale up during high traffic and back down when it's quiet, without manual intervention.

Triggered by events:
Functions are invoked by events, like HTTP requests, messages, or file uploads. This makes serverless a natural fit for event-driven architectures, integrations, and automation flows.
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
layout: image-right
image: /SpringCloudFunction.png
---
# Spring Cloud Function in a Nutshell

<v-clicks>

- Cloud Agnostic

- Adaptable to Environments

- Spring Boot Features

</v-clicks>

<!--

- Cloud Agnostic: Enables apps to run across AWS Lambda, Azure Functions, or locally without code changes.
- Adaptable: Supports web endpoints, streaming, or background tasks with the same function.
- Spring Boot Features: You get dependency injection, config, and metrics even in FaaS.
- GraalVM: Optimizes cold start, though AWS SnapStart or Azure Premium are alternatives.

- Call AWS and Azure health check
-->

---

# AWS Function Code Examples

Insure to include spring cloud function adapter in infrastructure layer dependencies.

<v-clicks>

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

</v-clicks>

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

# Azure Function Code Examples

Insure to include spring cloud function adapter in infrastructure layer dependencies.

<v-clicks>

### build.gradle.kts in Azure infrastructure module:

```kotlin
implementation("org.springframework.cloud:spring-cloud-function-adapter-azure:4.2.2")
```

### Azure function

```kotlin
@FunctionName("MockNestForwarder")
fun forwardToMockNest(
    @HttpTrigger(
        methods = [HttpMethod.POST, HttpMethod.GET, HttpMethod.PATCH, HttpMethod.PUT, HttpMethod.DELETE],
        authLevel = AuthorizationLevel.FUNCTION,
        name = "request", route = "mocknest/{*route}"
    ) request: HttpRequestMessage<String>,
    context: ExecutionContext,
): HttpResponseMessage {
    return buildResponse(request)
}
```

</v-clicks>

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
preload: false
---

# Spoiler Alert 🚨

Hello World is already racing —  
we’re not going to build it.  
We’re going to upgrade it.

<div class="w-full relative mt-6">
  <div class="relative w-80 h-80">
    <img
      v-motion
      :initial="{ x: 800 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://cdn-4.motorsport.com/images/amp/63vxMQEY/s1000/formula-1-red-bull-racing-laun-2.jpg"
    />
  </div>

  <div
    class="text-5xl absolute bottom-16 left-40 text-[#2B90B6] z-10"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: -7, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Fast-forward to
  </div>

<div
  class="text-5xl absolute bottom-16 left-[calc(50%+12rem)] z-10 font-mono text-pink-600 tracking-tight"
  v-motion
  :initial="{ y: 100, opacity: 0}"
  :enter="{ y: -4, opacity: 1, transition: { delay: 3500, duration: 1000 } }"
>
  Hello World
</div>


</div>

<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}

setTimeout(() => {
  const el = document.getElementById('replaceK');
  if (el) el.style.opacity = "0";
}, 3500);
</script>

<!--

Because let's face it — anyone can deploy Hello World.

But before we move on, let’s take a moment to understand what that deployment is actually doing.

Then we’ll live-code how to use Clean Architecture with Spring Cloud Function to inject real business logic into both AWS and Azure — while keeping cloud-specific code separate from the core logic.

-->

---
class: flex flex-col justify-center items-center h-[100vh] space-y-4
---

# Clean Architecture for Serverless

<img src="/CleanArch.png" alt="Clean Architecture" style="width: 80%; height: auto;" />


<!--
- **Separation of Concerns**: Different aspects of software development (use case business rules, domain logic,
  integration logic) are isolated from each other.

- **Independent Layers**: Changes in one layer (like switching cloud providers) should not affect other layers.

- **Testability**: Because of the clear boundaries and interfaces between layers, testing becomes straightforward.

- **Minimizing Cloud Lock-in**: Easily switch between AWS, Azure, or others, with business logic unaware of the
  underlying cloud provider.


-->

---

# Clean Architecture - with gradle modules

```css {all|1-3|4-5,8-9|4,6,8,10}
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


## Solution Design

<br>

<img src="SolutionDesign.png" alt="Solution Design" class="max-w-[60%] max-h-[60vh] object-contain mx-auto" />

<!-- 
(Call Azure)

So wer are not using a Hello world, however the use case is still simple, we hve some business logic which requires some persistence, and we are using a cloud specific service for this persistence. In AWS we will use S3, and in Azure we are using Blob Storage. These are exactly what we need to store our mock configuration.
-->

---

# 🧑‍💻 Live Coding: From Hello World to Business Logic

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

- build, commit and push


-->

---

# Terraform CDK

<v-clicks>

- **Multi language support**

- **Multi-Cloud Compatibility**

- **Reusability**

- **Predictable Changes**

</v-clicks>

<!--
**Multi language support**: Utilize familiar programming languages like Kotlin, Java or TypeScript for infrastructure
  code.

- **Multi-Cloud Compatibility**: Define and provision infrastructure seamlessly across multiple cloud providers like
  AWS, Azure, and Google Cloud.

- **Reusability**: Leverage constructs (modules) to create reusable, shareable, and composable components.

- **Predictable Changes**: Employ `cdktf diff` and to understand the changes.

-->

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

🧹 **Cloud Anostic Business Logic with Clean Architecture and Spring Cloud Function**  

📦 **Structure with Gradle Modules**  

🌍 **Deploy and Run Anywhere with Terraform CDK**  

🛠️ **Kotlin Everywhere**

</v-clicks>

<!-- 

🧹 **Keep Business Logic Clean and Cloud-Agnostic**  
Use Clean Architecture and Spring Cloud Function to isolate business logic from cloud-specific code.

📦 **Use Gradle Modules for Clear Structure**  
Separate your domain, application, and infrastructure layers to enforce clean boundaries.

🌍 **Deploy Anywhere with Terraform CDK**  
Once isolated, your logic can be reused across cloud platforms like AWS and Azure.

🛠️ **Kotlin Across Your Stack**  
Use Kotlin not just for business logic, but also for infrastructure (Terraform CDK) and build scripts (Gradle).

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
