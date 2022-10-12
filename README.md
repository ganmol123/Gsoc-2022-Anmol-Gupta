# Gsoc-2022-Anmol-Gupta

This repository contains all the work done under the Google Summer of Code Program, 2022 for the Mifos Organization for the project titiled 'Integrating an IAM system with Payment Hub'.

## Project Abstract

The Problem: There's no IAM System for Payment Hub.
Why is this a problem? As of now, we have to deal with authenticating each user for every application. Integrating an IAM with Payment-Hub will help balance security with efficiency, giving customers quick access to their finances while barring anyone else. As other digital technologies become increasingly agile, customers become more accustomed to speed, making these considerations more critical. Also, with the rise in remote access to the system, we must have a secure way to authenticate and authorize users. If every employee had access to all data, one breached account could spell disaster for an entire Financial Organization. IAM brings in the segmentation and privilege restriction aspects which is necessary as remote access expands. The agility benefits of segmentation and digital identities make IAM the ideal solution. Goal: Integrating an Identity and Access Management system (Anubis/Keycloak) to Payment-Hub. Project Impact: Currently, there is no Identity manager in Payment-Hub and users are authenticated only using basic auth. Integrating an IAM System(Keycloak) with Payment Hub will have the following major impacts: - Single Sign-On capability: There will be unique sign-on credentials for access to any system, network or service. Keycloak can be used to integrate this. - Multi-Factor Authentication: It will involve two or more forms of login credentials for information access. - Privileged Access Management: We would be able to bound users to a set of privileges/access. This will provide a secure way to monitor access control with more visibility. - Securing APIs: IAM system will also help secure APIs and API Keys. Deliverables: The deliverables are divided into three main tasks, namely, - Token Introspection - Scope Check - OAuth MTLS.

## Things Accomlished

Even though I was not able to accomplish majority of the initial goals, I have documented all the work done including proof of concept for authentication and authorization in PH-EE and configuration values for authentication in PH-EE. They are divided into three folders with detailed explanation in their README files. The three major task accomplished are as follows:

- Authentication POC: A detailed explanation to protect and secure the API resource using Kong, Konga and Keycloak.

- Authentication Configuration for PH-EE: Steps followed and configuration values in order to implement authentication in PHEE. It has three main steps: Keycloak Deployment, Kong API gateway woith Konga and Authentication with Kong and Keycloak.

- Authorizatiom POC: This is a simple example of Authentication and Authorization connecting the Java Application with Keycloak. Here I have a simple case study as an example and it discusses the two main approached of authorization using Keycloak.

## Conclusion

It was an enriching experience working for Google Summer of Code with Mifos Initiative. I would first most would like to thank my mentor Subham Pramanik for this opportunity. I got all the support and help needed from the organization. Weekly check-In meetings were organized by the mentiors and they were very helpul to discuss and resolving the blockers. Due to some unavoidable circumtances I need an extension which was granted to me by the organization.

## Links

- [Project Proposal](https://summerofcode.withgoogle.com/media/user/ced599b147f0/proposal/gAAAAABjRtb6FbdGd2qdwkkZzY33ueqwmR6JJVsrn1VlCP_lg9ajY9RzDbWjEM-wK7irTCBIj-qNNo91lnCrRXtmRJX9GgG3UjbKQZB4fImWE4HaV9Frl_Y=.pdf)
- [Project Link on Gsoc Webiste](https://summerofcode.withgoogle.com/proposals/details/VXA0vKhv)
