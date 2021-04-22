# Using API Manager to Solve a Real-world Problem

# Introduction

Union Station is a major multimodal railway transportation hub. It is one of the busiest stations in the country and serves more than thousands of passengers a day. The train shed, platforms and tracks are owned by GOGO transit, and they operate the station. Trains are owned by the companies named Quantis, ColTrain and RailCo. To provide a digital ecosystem, all four companies are planning to develop their day to day business operations with WSO2 technology. These development ranges from providing different kinds of APIs to external/internal users, providing real time notifications, stream data processing, integrating with partners/external systems etc.

This tutorial will demonstrate how WSO2 technology can be used to cater to their different  requirements.

# Background and Setup

# Create REST API using OpenAPI definition

###User Story: 

**Coltrain is one of the railway companies that is partnered up with GOGO Train to provide better service to their customers. Coltrain already has some internally managed APIs deployed in-house and these are managed by their internal development team. One of the APIs is train schedule retrieval API which is intended for the public community to get the Coltrain schedules. Currently, this API is exposed for the public and Coltrain development team faces challenges in maintaining and handling high load for the API.**


By exposing this API through WSO2 API Manager, Coltrain expects to get the full benefits of an API Management solution such as API lifecycle management, security, throttling,etc. and decouple the maintenance overhead from the internal teams.


# Engage Access control to the API

# Implementing an API

#### User Story

**RailCo has a partnership with Telecom companies to sell their train tickets as Value added services. Currently RailCo has partnerships with three such companies (Nexus, Wishque & Tenet). RailCo has a requirement to gather the stats of bookings on a daily basis from these companies. All 3 companies are exposing this data but they are exposing it in different formats. RailCo wants to develop a service which aggregates all this data and exposes it as a single API. _They first decided to create their own backend to call each of these APIs internally and write the logic to consume each service to generate the required outcome. But quickly understood the complexity of development tasks they have to undergo. For example, each of these external services uses different protocols and message formats. This would require special skills for their development teams and would increase the maintenance overhead as well.  _RailCo identified that using WSO2 API Manager and WSO2 Micro Integrator, they could do this integration in no time and expose the API with additional QoS features.**

**_Time to Complete : ? mins_**

There are three different sample backends implemented which provide the metrics in different formats. When you invoke them directly, you can view metrics related to one Telecom company. 

```
curl -X GET 'http://localhost:8082/telecom-rest-service/nexus/v1/metrics' 

```

With Micro Integrator and Integration Studio, we can implement the business logic to call these three backends, aggregate the response and present it to the client as one response.

<img src="{{base_path}}/assets/img/tutorials/user-scenario/scenario_implement_api.png" alt="Implement API" title="Implement API" width="60%" />

To develop a service in Micro Integrator, you need to use Integration Studio. While developing you can try it out in the Embedded Micro Integrator inside the Studio. Once the development is complete you can export it as a Compose Application and add it to the Micro Integrator runtime.

<img src="{{base_path}}/assets/img/tutorials/user-scenario/integration_studio_view.png" alt="Integration Studio View" title="Integration Studio View" width="60%" />

Here, for simplicity, the service is already created and exported as a Composite Application and added to the Micro Integrator instance in the tutorial setup. You can test the Micro Integrator service by invoking it directly.

```
curl -X GET 'http://localhost:8290/metrics' 
```


If the service catalog is enabled in Micro Integrator, it will automatically push the API artifacts, Swagger definition to the API Manager during startup, which you can view in the API Manager Service Catalog.

To enable the service catalog, uncomment the following section in the Dockerfile found inside _&lt;REPO_HOME>/dockerfiles/micro-integrator/._

`COPY deployment.toml ${WSO2_SERVER_HOME}/conf/`

This will add the new config file to setup. To apply the config, restart the Micro Integrator by running the following command. This will restart the Micro Integrator container.

_`docker-compose up -d --build mi-runtime`_

Once started, you can observe the following log in Micro Integrator.

`{ServiceCatalogUtils} - Successfully updated the service catalog`

You can view the API entry in the API manager by visiting the following URL. (Login as jill@railco.com)

[https://localhost:9443/publisher/service-catalog](https://localhost:9443/publisher/service-catalog)

<img src="{{base_path}}/assets/img/tutorials/user-scenario/service_catalog_view.png" alt="Service Catalog View" title="Service Catalog View" width="60%" />

<img src="{{base_path}}/assets/img/tutorials/user-scenario/service_catalog_detail_view.png" alt="Service Detailed View" title="Service Detailed View" width="60%" />

In this setup, the API is already deployed from the publisher and you can view in the DevPortal. 

1. Go to [https://localhost:9443/devportal/](https://localhost:9443/devportal/) Dev portal and select **RailCo** tenant domain. This will redirect you to RailCo’s developer portal.
2. Sign in with a RailCo tenant, developer portal user. (tom@railco.com)
3. Click on TelecomMetricsAPI and go to Subscribe tab and subscribe using the Default application and generate the access token.
4. Once you generate an access token, you can try out the following Curl Request.

    ```
    curl -k -X GET 'https://localhost:8243/t/railco.com/operations/bookings/1.0.0/' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <Access Token>'
    ```

# Signing up a new user

# Getting the developer community involved

# Integrating with Data sources

# Analytics

# Throttle out API requests

# Realtime data with Websocket API

# Notifications using webhooks

# GraphQL support

# Guaranteed message delivery

# Integrate with services via Connectors

# External keymanager support
