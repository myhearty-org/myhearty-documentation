<div align="center">
  <a href="https://github.com/myhearty-org/myhearty">
    <img src="images/myhearty-logo.svg" alt="MyHearty Logo" width="100" height="100">
  </a>
  <h2 align="center">MyHearty</h2>
  <p align="center">
    A one-stop charity website to fundraise, donate, volunteer and apply for aids
    <br />
    <br />
    <a href="https://www.myhearty.my">Website</a>
    ·
    <a href="https://dashboard.myhearty.my">Dashboard</a>
  </p>
</div>

---

# MyHearty: Documentation

This repository contains the project overview, technical documentation and architecture diagrams of MyHearty. The documentation serves as a permanent record for the project implementaton.

## About The Project

MyHearty is a one-stop, centralized charity website for people to fundraise, donate, volunteer and apply for aids. The motivation behind this project stems from the idea of integrating fundraising, donation, volunteering and aiding functions into a single platform with API access. This project aims to connect various parties that are involved in charity via a centralized platform and provide open charity data via an API.

## Big Picture

Figure 1 shows a big picture overview of MyHearty. The proposed solution consists of 3 parts: MyHearty Website, MyHearty Dashboard and MyHearty API Server. There are 6 types of users involved, which are charities, organizations, donors, volunteers, receivers and third-party developers.

| <img src="images/big-picture.svg" alt="MyHearty Big Picture"> |
| :-----------------------------------------------------------: |
|        **Figure 1: Big picture overview of MyHearty**         |

Via MyHearty Dashbaord, both charities and organizations can create volunteer event and aid pages. However, only charities can create fundraising campaigns pages to accept donations from the public. For MyHearty Website, donors can donate for a fundraising campaign, volunteers can register for volunteer events and receivers can apply for available charitable aids. Both the dashboard and the website are powered by MyHearty API that serves JSON responses to the frontend API clients. The API server also allows third-party developers to access charity data, thus opening up chances for developers to build custom integrations into their platforms. Besides, Stripe will be used as the payment processor for donations made on MyHearty Website.

## High-Level Architecture

Figure 2 shows the high-level architecture diagram of MyHearty. The high-level architecture diagram consists of 3 parts: the frontend app, the backend services and the payment processor. The frontend apps are deployed as [Next.js](https://nextjs.org) apps on [Vercel](https://vercel.com) to leverage the framework’s CSR and SSR features and also Vercel’s global edge network capabilities.

| <img src="images/high-level-architecture.svg" alt="MyHearty High-Level Architecture" width="75%" height="75%"> |
| :------------------------------------------------------------------------------------------------------------: |
|                                 **Figure 2: High-level architecture diagram**                                  |

For the backend, all services are deployed as multi-container Docker applications via [Docker Compose](https://docs.docker.com/compose) on the [Exabytes VPS](https://www.exabytes.my/servers/ssd-vps). There are 4 backend components: NGINX web server, Rails API server, PostgreSQL database and Typesense search engine. The [NGINX web server](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy) acts as a reverse proxy server that directs client requests to the appropriate backend services based on the API URL paths, which, in this case, refer to the Rails API server and the Typesense search engine. The [Rails API server](https://guides.rubyonrails.org/api_app.html) is responsible for hosting MyHearty API that handles requests from the Next.js apps. It communicates with the [PostgreSQL database](https://www.postgresql.org) for access and retrieval of the app data. The [Typesense search engine](https://typesense.org) is used to offer typo-tolerant instant search for resources hosted on MyHearty, which include fundraising campaigns, volunteer events and charitable aids. The API server will index the resources into Typesense collections every time a new resource is published or updated on MyHearty.

[Stripe](https://stripe.com) is used as the payment processor for donations made on MyHearty. There are 2 services involved: Stripe Connect and Stripe Checkout. [Stripe Connect](https://stripe.com/docs/connect) is used to onboard organizations that want to publish fundraising campaigns on MyHearty by collecting their business information, which include business name, location and bank account information. This avoids the need for MyHearty to store sensitive data that often require strict data compliance process. Besides, [Stripe Checkout](https://stripe.com/docs/payments/checkout) provides a hosted payment page that can be customized to securely accept online payments from donors. It supports credit card, debit card, FPX and some digital wallet payments.

## Detailed-Level Architecture

| <img src="images/detailed-level-architecture.svg" alt="MyHearty Detailed-Level Architecture"> |
| :-------------------------------------------------------------------------------------------: |
|                       **Figure 3: Detailed-level architecture diagram**                       |

## Database Diagram

| <img src="images/myhearty-db.svg" alt="MyHearty Database Diagram"> |
| :----------------------------------------------------------------: |
|                   **Figure 4: Database diagram**                   |
