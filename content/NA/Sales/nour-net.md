# nour.net.sa
Goal: to clarify questions regarding scope of work and requirements:

### Request:
We want to upgrade existing services from `nour.net.sa` (provider), we want to work with UX of the portal and also extend functionality.

> Scope of Work for Replacing and Revamping NourNet’s eServices Portal
> 
> ### Project Overview
> NourNet, one of the six accredited registrars nominated by Saudi NIC, is seeking to completely replace and revamp its current portal, eservices.nour.net.sa, to enhance the user experience, improve system functionality, and allow for the resale of Saudi domain names. This project will ensure that the new portal meets modern UI/UX standards, complies with Saudi NIC requirements and integrates with multiple services and platforms.
> 
> ### Objective
> The new portal will not just upgrade the existing platform but replace it entirely with a fully redesigned and redeveloped system. The goal is to provide a seamless domain registration experience for both personal and business users, with integrated payment gateways, verification systems and NourNet cloud platform.
> 
> ### Key Features and Integrations
> 
> #### UI/UX Overhaul & Complete Replacement
> The existing portal will be replaced with a new platform built from the ground up, incorporating modern UI/UX best practices.
> Design an intuitive, mobile-optimized interface with easy navigation to enhance user satisfaction.
> Focus on simplifying the registration process for Saudi domains while ensuring regulatory compliance.
 > #### SEO Optimization
>The new portal will be optimized for Search Engine Optimization (SEO) to improve visibility and rankings in search engines.
> Implement all on-page, technical, and off-page SEO activities such as metadata, keywords, headers, fast loading speeds, mobile-first design principles, sitemaps, robots.txt, etc. to attract more organic traffic.
> #### Saudi NIC Integration
> Full integration with Saudi NIC to enable domain registration approval in compliance with local regulations.
> Nafath Integration (via IAM by ELM)
> Integrate with Nafath for personal identity verification through the IAM service.
> Wathiq Integration
> Verify Commercial Registration (CR) information via Wathiq for companies registering domains.
> Payment Gateway Integration with Hyper Pay and Digital Cash (Sadad)
> Integrate with MADA, Visa, MasterCard, and Sadad to offer multiple secure payment options.
> #### Unifonic SMS Gateway Integration
> Enable two-factor authentication (2FA) through SMS verification using the Unifonic gateway for enhanced security.
> Ensure that users receive verification codes during domain registration and critical account transactions.
> #### Email Gateway Integration
> Integration with an email gateway to send secure verification codes for transactions and user authentication.
> #### Cloud Service Integration & Promotion
> The new portal will integrate with NourNet’s cloud platform to promote complementary services such as DNS Hosting, Web Hosting, Email as a Service, VPS, etc.
> Ensure seamless bundling of services during domain registration to maximize customer engagement.
>
> Ensure full compliance with Saudi NIC and local regulations governing domain name registrations.
> All relevant documents and standards from Saudi NIC will be used to guide development.
> #### Competitor Analysis
> The following competitors provide benchmarks for design, functionality, and user experience in reselling Saudi domains, with Dnet and Hawsabah noted as top performers:
>
> - [Dnet](https://dnet.sa/)
> - [Hawsabah Cloud](https://cloud.hawsabah.sa/index.php/login)
> - [T2](https://t2.sa/)
> - [Sahara](https://sahara.com/)
> - [STC Cloud](https://cloud.stc.com.sa/)
>
> #### Support & Maintenance
>
> Ongoing technical support post-launch, including security updates, feature improvements, and performance monitoring.
> #### Conclusion
>This project is a complete replacement and revamp of the current eservices.nour.net.sa portal, aimed at delivering a user-friendly, secure, and fully integrated platform for Saudi domain registration while cross selling NourNet cloud services.
### Questions:
- could we obtain access to existing customer panel? We were not able to break throw SMS verification limited to Saudi phone number.
	- Should Provider work only with Saudi citizens, or it should be international service? 
	> Provider oriented on local citizens and primary goal is to sell local country specific domains.
- the main service that we should provide is domain registration?
	- does SA zone registrar provides some test or stage environment to test [EPP](https://datatracker.ietf.org/doc/html/rfc5731.html) integration with them?
> It will be clarified by customer
- Could we obtain access to existing admin panel?
	- We need to understand how you want to work with prices
	- Also we need to understand, is your current billing solution based on [WHMCS](https://www.whmcs.com/) or something similar, or you have some requirements that could not be covered by them, and we have to build all from scratch? From what we see, there is a very strict KYC requirements plus custom payment solution
> current solution build from scratch, without any kickstart solution in a base. The pricing model is pretty simple — discounts on forepay and promo codes
- Do we need to support recurrent payments? [HyperPay](https://www.hyperpay.com/) supports it, but [Sadad](sadad.com) — seems to be missing that feature. https://dg-cash.com/contact/
> We need both systems. HyperPay for local cards, and Sadad for invoicing. They have a bit different flows (one is Push, another is Pull), that's why we will not cover them by one umbrella.
- In requirements we see: “Ensure seamless bundling of services during domain registration”. Does that means that we should have some interop with other service portals to sell them together? How it should behave, would be great to have better explanation
> At current step there is no request for direct integration. If users wants additional services from as we will put him on a track in a different product manually. All we need from technical site is to notify about intent to buy a new product.
- we will need help to integrate with Nafath. [Nafath](https://www.iam.sa/sso/) is a government based SSO in SA, that allows to identify any citizen. Like E-passport. We will need some accounts to testing it, because we are not citizens of country.
> Accounts will be provided by client
- providing NS services is also part of our portal, what existing solution used to manage NS server settings?
> Currently NS servers are not part of the product. We can keep them out from scope from now.
- What should we do with existing customers? Should we migrate existing accounts? Should we migrate purchase history, existing products, etc? If yes — then we would need some documentation on existing system. Have you any?
> could be elaborated after getting access to current system
- Have you any docs on existing system? It would be nice to get some product and tech docs, if they are exist
> no docs
- Is providing email services also part of the portal? Should we manage dkim, dmarc etc from our side?
> no, currently some solution based on MS services used to provide email services as a separate service.



## PreEstimation

The solution architecture for the revamp of the NourNet eServices portal can be divided into the following key components:
#### User Registration and Authentication
There are two options:
- Implement integrated Single Sign-On (SSO) across all products using tools like [Keycloak](https://www.keycloak.org/) or [AuthKit](https://www.authkit.com/) for robust security and scalability. Keycloak is widely supported and can cater to extended integration needs. 
- Alternatively, for solutions focusing solely on the portal, consider lightweight authentication platforms like [Better Auth](https://www.better-auth.com)
#### KYC Process Management
- **Client-Side:** Handle user verification procedures partially with existing third party providers. Collaborate closely with providers to document and manage the KYC data, including handling NIC (Network Information Center) data.
- **Admin-Side:** Develop a secure admin panel for KYC status monitoring and document exchange. This ensures compliance and efficiency, allowing the management of multiple NIC handles under a single user account. From regulatory side we have to make KYC for every new handle, not only for initial registration.
#### Domain Shop Functionality
- Develop a streamlined process where users can search for domain availability, select additional services, and complete purchases with ease.
#### Payment System Integration
- Implement both real-time credit card processing and delayed invoice-based payment models. It includers integration with local services like Hyper Pay and Sadad to ensure comprehensive coverage.
#### EPP Integration
- Conduct thorough testing with the Saudi NIC EPP (Extensible Provisioning Protocol) system. Be prepared to adapt to any inconsistencies or deviations in protocol standards through close interaction with testing environments. If now testing environment present — it could cause big delays in implementation.
#### CRM Integration
- Export customer data to a CRM solution for holistic customer management and analytics. If no preferred CRM exists, consider starting with [Hubspot](https://www.hubspot.com/) for its user-friendly features and integration capabilities. CRM will be a master data storage that collects customer data from all yours products. 
#### Upselling and Cross-Selling Features
- Implement recommendation systems and pop-ups to promote related services. Define a flexible integration system to accommodate future partnerships and offerings.

Estimations:
- Registration: SSO, up to one month.
- Registration: without SSO, up to two weeks
- KYC process (client side): near one month
- KYC process (admin side): few weeks
- Shop functionality: up to month
- Payment system integration: up to month
- EPP integration: up to two month
- CRM integration: few weeks
- Up sale functionality: month or two

Estimation, $ 


| Developer        | Hourly rate  | Hours per Month  | Costs        |
|------------------|--------------|------------------|--------------|
| BE               | 50           | 160              |  8,000.00    |
| BE               | 50           | 160              |  8,000.00    |
| FS               | 40           | 160              |  6,400.00    |
| FS               | 40           | 160              |  6,400.00    |
| DevOPS           | 68           | 40               |  2,720.00    |
| TechLead         | 75           | 40               |  3,000.00    |
| PM               | 25           | 160              |  4,000.00    |
| Tester           | 18           | 40               |  720.00      |
| Costs per month  |              |                  |  39,240.00   |
|                  |              | 4m               |  156,960.00  |
|                  |              | 8m               |  313,920.00  |


#### Cover letter
The KYC process should be discussed with the registrar. According to the documentation, they require full KYC for each NIC handle. However, there may be instances where this requirement can be bypassed. This has a significant impact on the user flow, and it needs to be clarified before coding begins, during the BA phase.

Additionally, if there is no test environment or test mode for the EPP provided by the registrar, we might need to allocate extra resources to purchase or transfer some real domains for development purposes and also an extra time.

Regarding DNSSEC, it is supported by the registrar. However, we will exclude it from the current estimation because we need to complete the EPP integration first. Implementing DNSSEC will be a subsequent step for product improvement.