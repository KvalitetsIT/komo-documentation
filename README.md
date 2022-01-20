# KoMo-documentation

## Introduction

These pages contain the overall technical documentation for the services in the KoMo project. KoMo is short for "communication and monitoring" in danish "KOmmunikation og MOnitorering". 
The project is funded by the Central Denmark Region (RM) and development is carried out RM and KvalitetsIT.

This repository contains the overall technical design and architectural documentation for the project as well as an overview of the constituent components.


## Design principles
The project uses the following design principles:

* Cross-cutting concerns are externalized 
  - Login (patient and employee). User authentication is done by external IdPs. Standards used OIDC or SAML.
  - Auditlog are done by using a external auditlog service.
* Use dedicated backends for each each front end - backend for frontend(BFF).
* Standard FHIR model used when possible.


## Usecases
All usecases are documented the most recent usecase document can be send on request.

<<Må vi lægge usecase dokumentet ud?>>


## Services
Services are created, testet and build in independant git repositories 

| Service       | Description   |
| ------------- |-------------|
| [Patient-web](https://github.com/KvalitetsIT/hjemmebehandling-patient-web)       | The web application for patient |
| [Medarbejder-web](https://github.com/KvalitetsIT/hjemmebehandling-medarbejder-web)     | The web application for employee(medarbejder) |
| [Patient-BFF](https://github.com/KvalitetsIT/hjemmebehandling-medarbejder-bff)  | Dedicated API for patient |
| [Medarbejder-BFF](https://github.com/KvalitetsIT/hjemmebehandling-medarbejder-bff)  | Dedicated API for medarbejder |
| [BackendApi](https://github.com/KvalitetsIT/hjemmebehandling-hapi-fhir-server)  | A instance of a HAPI FHIR server |
| [Web-shared](https://github.com/KvalitetsIT/hjemmebehandling-web-shared) | Shared web components|

## Technical setup

The backend business functionality is exposed to external clients (apps) through patient and employee (health professional) backend-for-frontend (BFF) services. BFFs and input services encapsulate the central microservice-based infrastructure. The microservices inside the backend convert, store, process and exhibit incoming data. 

The main flow is
1. All calls for resources (javascript, html etc) or data (api) are receieved by the patient or medarbejder-web services. The loginproxies will force a login before resources or data are returned. 
2. Api calls are proxied the BFFs. All Apis are Rest. 
3. The BFFs uses a FHIRclient to communicate with the backend. The medarbejder BFF uses external services. These are described under intergrations.
4. The backend are a standard FHIR server fetches and saves data in a Maria Database. The backend are build from the [HAPI FHIR server](https://hapifhir.io/)

![Overall business architecture](images/services.png)


## Integrations
KoMo integrates with the following systems

| Integration   | Description   | Standards | Documentation |
| ------------- |-------------| -----|-----|
| BSK (BrugerStamdataKatalog) | Is used for employee login. A JWT token containing userinformation and roles is parsed to the backend where userinformation are handled and access validated.  | OIDC / SAML | [DIAS documentation](https://git.base.dias.rm.dk/DIAS/dias-app-chart#oauth2-oidc) - login is required to view documentation  |
| CustomLogin | Is used when a patient login to the system. Patient users are automatically created in the CustomUser IdP using a Rest interface. It is possible to reset the password from the employee application. |OIDC and Custom Rest| [DIAS documentation](https://git.base.dias.rm.dk/DIAS/dias-custom-user-service) - login is required to view documentation |
| Audit service | Audit service is called from employee-bff when the employee reads or writes patient data | Custom Rest | [DIAS documentation](https://git.base.dias.rm.dk/Tenants/flux-kit/src/branch/master/README.md#audit-logs) - login is required to view documentation |
| CPR service | Cpr service is used to fetch masterdata for patient. Data a fetched from a Cpr number | Custom Rest | [DIAS documentation](https://git.base.dias.rm.dk/DIAS/cpr-service) - login is required to view documentation |

## Model
The model is based on [FHIR](http://www.hl7.org/fhir/). The Hapi FHIR is used for implementing the FHIR server.

The following FHIR Resources are used
 * carePlan 
 * patient
 * practitioner
 * organization
 * planDefinition
 * questionnaire
 * questionnaireResponse
 * searchParameter

The following extensions are used
...

## Alarms, triage and other business rules


