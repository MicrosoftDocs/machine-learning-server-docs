---

# required metadata
title: "About DeployR - DeployR 8.x "
description: "DeployR introduction and overview."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "11/10/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "deployr"
#ms.custom: ""
---

# About DeployR

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../whats-new-in-r-server.md#8vs9))

>Looking for docs for Microsoft R Server 9? [Start here](../what-is-operationalization.md).


DeployR is an integration technology for deploying R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. DeployR turns your R scripts into [analytics web services](#analytics-web-service), so R code can be easily executed by applications running on a secure server.

Using analytics web services, DeployR also solves key integration problems faced by those adopting R-based analytics alongside existing IT infrastructure. These services make it easy for application developers to collaborate with data scientists to integrate R analytics into their applications without any R programming knowledge.

DeployR Enterprise scales for business-critical applications and offers support for production-grade workloads, as well as seamless integration with popular [enterprise security solutions](../operationalize/configure-authentication.md) such as single sign-on (SSO), Lightweight Directory Access Protocol (LDAP), Active Directory, or Pluggable Authentication Modules (PAM).

## Basic Workflow

This diagram captures the basic workflow used by data scientists and application developers when collaborating on the delivery of solutions powered by analytics Web services.

![DeployR Workflow](./media/deployr-about/DeployRWorkflow.png)

## Workflow In Action

The workflow is simple. A data scientist develops an R script (using standard R tools) and publishes that script to the DeployR server, where it becomes available for execution as an analytics web service. Once published, R scripts can be executed by any authorized application using the DeployR [(API)](deployr-api-reference.md).

The following real-world scenario demonstrates the key concepts introduced in preceding workflow diagram.

While this scenario is provided as an example only, it highlights how DeployR acts as a effective bridge between data scientist and application developer workflows. By supporting a clean separation of concerns, DeployR delivers arbitrarily sophisticated R analytics to any network-enabled application, system or software solution.

>**The Scenario:** As a solutions provider at an insurance company, you have been tasked with enhancing an existing customer support system to deliver real-time, high-quality policy quotes to customers who contact your customer support center.
>
>**The Plan:** To deliver this solution, you decide to leverage the power of R-based predictive analytics with DeployR. The solution relies on a real-time scoring engine architecture to rapidly generate and return accurate scores to your existing customer support system. These scores can then be used to drive the customer experience to a successful conclusion. At a high-level, such a DeployR-powered solution can be realized as follows:
>
>1. **Produce:** Data scientists begin by building an appropriate predictive model and scoring function for insurance policy quotes using their existing analytics tools and data sets.
>
>2. **Upload:** Once the model and scoring function are ready, the data scientist uploads these files to the DeployR repository via the Repository Manager. Uploading the scoring function as an R script will automatically turn it into an analytics Web service.
>
>3. **Verify:** As a final step, the data scientist should test and verify the scoring function against a live DeployR server via the Repository Manager.
>
>	>Handoff From Data Scientist to Application Developer  
>
>4. **Integrate:** Application developers choose and download their preferred client application integration tool: RBroker framework, client library or working with the raw API.
>
>5. **Test:** With their tool selected, application developers implement the integration between the customer support system and the DeployR-powered analytics Web service. This can be an iterative process with testing at each stage of development.
>
>6. **Deploy:** Once application developers are confident their integration is completed, tested, and verified, it is time to deploy your enhanced customer support system to your live production environment.

## Roles & Responsibilities

In this section, you will find a description of the roles and responsibilities of each actor in DeployR:

-   [Data scientists](#data-scientists)
-   [Application developers](#application-developers)
-   [System administrators](#system-administrators)

This diagram depicts how data scientists and application developers collaborate when working with DeployR.

![story](./media/deployr-about/DeployRWorkflowStory.png)

### Data Scientists

#### Role

Data scientists, sometimes referred to as R programmers, typically focus on developing analytics solutions using their existing analytics tool chain—R IDE, StatET, and others. To maximize the impact of these data scientists, DeployR is designed to encourage minimal change in this workflow. Therefore, data scientists can remain focused on creating the R code, models, and data files necessary to drive your analytics solutions without having to concern themselves with how these outputs are eventually used by application developers in their software solutions.

In DeployR, there is but one tool for the data scientists -- the [Repository Manager](#deployr-repository-manager). The Repository Manager is a web-based tool that serves as a bridge between the data scientist’s work and the deployment of that work into the DeployR repository. When the data scientist uploads his or her R scripts, models, and data files into the Repository Manager, they turn into [analytics Web services](#analytics-web-service) that can be consumed by any software solution. Once deployed, application developers can create DeployR-powered client applications and integrations.

#### Responsibilities

With DeployR, the recommended steps for a data scientist are both simple and familiar:

1. Develop your R scripts and other analytics with portability in mind
1. Test those analytics inside and outside of DeployR
1. Collaborate with application developers to deliver powerful R analytic solutions

Learn more in the [Getting Started Guide Data Scientists](deployr-data-scientist-getting-started.md).

### Application Developers

#### Role

Unlike the data scientist who focuses solely on developing the R scripts, models and data files, the application developer does not need to know any R. Instead, this role is focused solely on integrating the output of the data scientists' work, [analytics Web services](#analytics-web-service) into their applications. The hand-off occurs in the [Repository Manager](#deployr-repository-manager). The separation of responsibilities between the data scientists and application developers is key to working effectively in the DeployR environment.

There are several client application integration tools available to application developers in DeployR. The [RBroker framework and the client libraries](deployr-tools-and-samples.md), which are provided in Java, JavaScript or .NET, greatly simplify the integration for those working in those languages. However, to integrate analytics Web services using other programming languages, the [API Reference](deployr-api-reference.md) guide details everything you'd need to know.

#### Responsibilities

Regardless of the integration tool you choose, or whether you are building a new application, system or solution versus enhancing an existing one, the basic steps to leveraging DeployR-powered analytics Web services inside any application remain simple and consistent:

1.  Consult with the data scientists responsible for developing the R analytics outputs in order to determine your application's analytics dependencies (inputs and outputs).

2.  Verify these dependencies in the Repository Manager by [testing](deployr-repository-manager-testing-debugging-scripts.md) the R scripts live on DeployR. For each script, you can inspect the API request and API response in the Artifacts pane to learn how your application needs to interact with that script. Note: Once the dependencies are in the DeployR repository, they become [analytics Web services](#analytics-web-service).

3.  Begin your integration by choosing a client application integration tool. [Download the RBroker framework or a client library](deployr-tools-and-samples.md) in either Java, JavaScript, or .NET. Or, if working in another language, read the [API Reference](deployr-api-reference.md) guide. To help you familiarize yourself with these tools, check out the tutorials and documentation provided on this site.

4.  Build or extend your application to take full advantage of DeployR-powered analytics Web services.

Learn more in the [Getting Started Guide for Application Developers](deployr-application-developer-getting-started.md).

### System Administrators

Not unlike the responsibilities typically associated with managing and maintaining other server software, DeployR system administrators are responsible for:

1.  [Provisioning suitable hardware](deployr-installation.md) in preparation for a DeployR install.
2.  Installing DeployR using [these instructions](deployr-installation.md).
3.  Customizing DeployR [server policies](deployr-admin-managing-server-policies.md).
4.  Creating and managing DeployR [user accounts](deployr-admin-console-user-accounts.md).
5.  Customizing DeployR [security policies](deployr-security.md).
6.  Monitoring and [maintaining](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing) your DeployR deployment.

[Consult the administration documentation](deployr-administrator-getting-started.md) that details the various DeployR tools made available to administrators. These tools simplify common and advanced administrative tasks.

>**DeployR Security**  
>DeployR supports a highly flexible, enterprise-grade security framework that verifies identity, enforces permissions, and ensures privacy.
>
>Identity is established and verified using many well-known authentication solutions. Basic authentication, using username and password credentials, is available to all DeployR installations by default. The DeployR Enterprise extends support for authentication by providing a seamless integration with established enterprise security solutions including CA Single Sign-On, PAM authentication, LDAP authentication, and Active Directory authentication.
>
>Learn more about authentication, access controls, and privacy with DeployR in our [Security](deployr-security.md) guide.

## Architecture

DeployR is a standalone server product, potentially sitting alongside but never directly connected with other systems, that can be deployed on-site as well as in private, public, or hybrid cloud environments.

Behaving like an on-demand R analytics engine, DeployR exposes a wide range of related analytics services via a [Web services API](deployr-api-reference.md).

![](media/deployr-about/deployr-architecture.png)

The fact that DeployR is a standalone product means that any software solution, whether it's a backend enterprise messaging system or a client application running on a mobile phone, can [leverage DeployR-powered analytics services](deployr-tools-and-samples.md).

DeployR Enterprise supports a scalable grid framework, providing load balancing capabilities across a network of node resources. For more on planning and provisioning your grid framework, see the [Scale & Throughput](deployr-admin-scale-and-throughput.md) guide.

## Glossary Of Terms

#### Analytics Web Service

In DeployR, we refer to any web service that exposes R analytics capabilities over the network as an analytics web service. While “web services” is commonly used in the context of browser-based web applications, these services—in particular, analytics web services—can just as easily be integrated inside desktop, mobile, and dashboard applications, as well as backend systems. For example, when you upload an R script into the DeployR Repository Manager, the script may then be executed as an analytics web service by any application with appropriate permissions.

*Note*: When you upload an R script into the [Repository Manager](#deployr-repository-manager), it becomes an analytics Web service that, with the appropriate access control, can be consumed by any application.


#### DeployR Administration Console

The Administration Console is a tool, delivered as an easy-to-use Web interface, that facilitates the management of users, roles, IP filters, the grid and runtime policies on the DeployR server. Learn more [here](deployr-admin-console-about.md).

<a name="deployr-repository-manager"></a>
#### DeployR Repository Manager

The Repository Manager is a Web-based tool that serves as a bridge between the data scientist's scripts, models, & data and the deployment of that work into the DeployR repository to enable application developers to create DeployR-powered client applications and integrations. Learn more [here](deployr-repository-manager-about.md).
