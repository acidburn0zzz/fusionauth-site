---
layout: blog-post
title: FusionAuth launches in the DigitalOcean marketplace
description: FusionAuth is now available as a 1-Click App in the DigitalOcean Marketplace. The pre-configured one-click option allows developers to add enterprise-quality authentication and authorization to their DigitalOcean-hosted applications in just a few minutes.
author: Emily Jansen
category: announcement
image: blogs/fusionauth-digitalocean/fusionauth-digitalocean-marketplace-header.png
tags: marketplace digitalocean kubernetes
category: announcement
excerpt_separator: "<!--more-->"
---


FusionAuth is now available as a 1-Click App in the [DigitalOcean Marketplace](https://marketplace.digitalocean.com/apps/fusionauth). The pre-configured one-click option allows developers to add enterprise-quality authentication and authorization to their DigitalOcean-hosted applications in just a few minutes. 


FusionAuth is the most complete auth server in the marketplace providing support for OAuth2, OIDC, SAML v2, social login, federated login, MFA, full-text search, password policies, WebAuthn, and other passwordless login options. 

<!--more-->

## How to Deploy FusionAuth in DigitalOcean

Go to the DigitalOcean marketplace listing [here](https://marketplace.digitalocean.com/apps/fusionauth). Then click the “Install App” button to deploy FusionAuth into your DigitalOcean Kubernetes cluster. FusionAuth requires a three-node deployment, with a minimum of 2.5 GB of total usable memory (out of 4 GB total) and 2 vCPUs per node. For more information on selecting the right machine type, node size, and count, please refer to Digital Ocean's documentation: [Choosing the Right Kubernetes Plan](https://docs.digitalocean.com/products/kubernetes/concepts/choosing-a-plan/).

{% include _image.liquid src="/assets/img/blogs/fusionauth-digitalocean/marketplace-listing.png" alt="The FusionAuth DigitalOcean Marketplace Listing." class="img-fluid" figure=false %}


## Getting started after deploying FusionAuth

After the FusionAuth stack is installed through the 1-Click app, you should: 

1. Run `helm list -n fusionauth` to confirm stack was deployed. You should have 3 deployments: `db`, `fusionauth`, `search`.
1. Check running pods and their status: `kubectl get pods -n fusionauth`
1. Connect to your cluster via the command line.

To visit FusionAuth, run this set of commands:

```bash
export SERVICE_IP=$(kubectl get svc --namespace fusionauth fusionauth -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

echo Visit http://$SERVICE_IP:9011/ to access the FusionAuth console.
```

Visit the URL printed after `Visit`.

Once you see the setup wizard in the browser, follow the [Setup Wizard](/docs/v1/tech/tutorials/setup-wizard) tutorial that will guide you through completing the initial FusionAuth configuration necessary prior to making API calls and beginning your integration.

## Enabling Identity Features

The community edition version of FusionAuth is deployed through the 1-Click application steps above. This includes authentication standards like [OAuth 2.0](/docs/v1/tech/oauth/), [SAML V2](/docs/v1/tech/samlv2/), OpenID Connect, and core features like SSO, MFA (one-time passcodes & app authenticator), [social logins](/docs/v1/tech/identity-providers/), access control, and user management. To access [premium features](/docs/v1/tech/premium-features/) like [MFA](/docs/v1/tech/guides/multi-factor-authentication#overview) (SMS & Email), [LDAP integration](/docs/v1/tech/connectors/ldap-connector), advanced registration forms, or engineer-led technical support reach out to our [sales team](/) to get the Starter, Essentials, or Enterprise plan enabled for your account. 

## Why Marketplaces

FusionAuth can be [installed on any server](/platform/self-hosting), anywhere in the world – and we want to make it super easy for developers to do just that, especially if they are using a cloud hosting provider.  DigitalOcean has a community of more than 3.5 million developers with many of them leveraging their managed Kubernetes clusters for development and production applications, making it a great fit for developers looking to deploy FusionAuth. This is our first (but not last)[ marketplace integration](/docs/v1/tech/installation-guide/marketplaces) for 2023. Stay tuned for more. 

