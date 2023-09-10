# Section 3.3: Monolithic Architecture

## 3.3.1 Monolith vs Microservices

### Monolith

**Definition:** in software engineering, a monolithic architecture refers to a software design pattern where all the components of the software system are interconnected and interdependent, rather than being broken down into separate, loosely-coupled modules. In a monolithic architecture, the software is essentially built as one cohesive unit.

| [![](https://mermaid.ink/img/pako:eNptkLFOAzEMhl8l8nx9gQwdECBuqIRoWVAWK3FpxJ1zJI4qVPXd8eWuXSBLYv_fb8X_BXwKBBYKfVdiT48RPzOOjo2eCbNEHydkMe-FssHS7n_Uvmn9X-UB_RdxmOVd4jREOS1QG7jZbtVlTc9CGb2UVetVWJ3WPKd8xhyKeTkcXs3b_NMiC7gyG8XnMUxn9UIHI-URY9DFLjPoQE40kgOrz0BHrIM4cHxVFKuk_Q97sJIrdVCngHLLAewRh3LvPoUoKd-b1MrdkmALsgNd-iOlm_H6Czl9dgc?type=png)](https://mermaid.live/edit#pako:eNptkLFOAzEMhl8l8nx9gQwdECBuqIRoWVAWK3FpxJ1zJI4qVPXd8eWuXSBLYv_fb8X_BXwKBBYKfVdiT48RPzOOjo2eCbNEHydkMe-FssHS7n_Uvmn9X-UB_RdxmOVd4jREOS1QG7jZbtVlTc9CGb2UVetVWJ3WPKd8xhyKeTkcXs3b_NMiC7gyG8XnMUxn9UIHI-URY9DFLjPoQE40kgOrz0BHrIM4cHxVFKuk_Q97sJIrdVCngHLLAewRh3LvPoUoKd-b1MrdkmALsgNd-iOlm_H6Czl9dgc) |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Figure 3.3.1.1: Basic working of a monolith                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

### Microservices

**Definition:** a microservices architecture breaks an application down into its individual components or services. Each service is a small, independent application that communicates with the others through well-defined APIs

{% hint type = "danger" %}

Microservices involve breaking down the **server-side** application

{% endhint %}

### API as a Backend

It is true that an API as a backend architecture decouples the user-interface component and the business logic component.

However, this is not considered a microservices architecture since the server-side application is still a monolith.

{% hint type = "info" %}

The API as a backend is also referred to as a **separated client-server** model

{% endhint %}

The following is an illustration of an SPA built using an API as a backend architecture

| [![](https://mermaid.ink/img/pako:eNptkMFqAyEQhl9F5tTC5gU8BLYkBwuBkBIoxcugk0a6q1sdDyXk3atusjm0XnTm_-Zn_C9ggiWQkOg7kze0cfgZcdRelDNhZGfchJ7FMVEUmNr9j6qapv4q_V69oPkibytRKvG0Cz4Mjs_PM92cV-t1GZdCeaaIhtNNU0V4WEjRv_bv4lCXTTwjD3VV2OpxoDQFn-husZgfJ4tMdU_oYKQ4orPl75cKauAzjaRBlqelE-aBNWh_LShmDm8_3oDkmKmD3HxuUYE84ZCW7tY6DnFpUit3c8gt6w5KLh8h3Aevv9v5fwA?type=png)](https://mermaid.live/edit#pako:eNptkMFqAyEQhl9F5tTC5gU8BLYkBwuBkBIoxcugk0a6q1sdDyXk3atusjm0XnTm_-Zn_C9ggiWQkOg7kze0cfgZcdRelDNhZGfchJ7FMVEUmNr9j6qapv4q_V69oPkibytRKvG0Cz4Mjs_PM92cV-t1GZdCeaaIhtNNU0V4WEjRv_bv4lCXTTwjD3VV2OpxoDQFn-husZgfJ4tMdU_oYKQ4orPl75cKauAzjaRBlqelE-aBNWh_LShmDm8_3oDkmKmD3HxuUYE84ZCW7tY6DnFpUit3c8gt6w5KLh8h3Aevv9v5fwA) |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Figure 3.3.1.2: SPA using an API as a backend                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

## 3.3.2 Inertia.js

What if we want an SPA experience without the need of creating a separate API?

**Inertia.js**: allows you to build an SPA without having to build a separate API. You can return Vue.js components directly from your Laravel routes, and Inertia.js takes care of updating the page without a full reload. 

The following diagram illustrates how inertia.js interacts with a Laravel monolith through Ajax to create the SPA experience.

| [![](https://mermaid.ink/img/pako:eNptkcFOwzAMhl8lyglExwPkMAkYhyIN0EovqBer8bqMNimJI4GmvTtuGibQlkMS5__y24kPsnUapZIBPyPaFlcGOg9DYwWPETyZ1oxgSdQBvYCQ1gtqmbTyXCktcgCTnLe3-3CO3UP7gVZP2NpZ1xvazVDKu1gu2VyxA6GHlrJBXbKQXZV486br0Adxt4cvsZkeFGgGM8N0zqNExfMlNAOLv84bpOhtEE_Vy7O4enDD6Cxy0TdiBQTX_3PwxanUetRAGMQrdCgLOaAfwGj-6cOEN5J2OGAjFW81biH21MjGHhmFSK76tq1U5CMWMian3BipttCH0-mjNuT86RBTuJ5bmjpbSP7ed-d-Lx5_AC6dp2w?type=png)](https://mermaid.live/edit#pako:eNptkcFOwzAMhl8lyglExwPkMAkYhyIN0EovqBer8bqMNimJI4GmvTtuGibQlkMS5__y24kPsnUapZIBPyPaFlcGOg9DYwWPETyZ1oxgSdQBvYCQ1gtqmbTyXCktcgCTnLe3-3CO3UP7gVZP2NpZ1xvazVDKu1gu2VyxA6GHlrJBXbKQXZV486br0Adxt4cvsZkeFGgGM8N0zqNExfMlNAOLv84bpOhtEE_Vy7O4enDD6Cxy0TdiBQTX_3PwxanUetRAGMQrdCgLOaAfwGj-6cOEN5J2OGAjFW81biH21MjGHhmFSK76tq1U5CMWMian3BipttCH0-mjNuT86RBTuJ5bmjpbSP7ed-d-Lx5_AC6dp2w) |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Figure 3.3.2.3: Inertia.js + Laravel + Ajax                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |