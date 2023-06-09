[AVM]: https://en.avm.de/
[Fritz!Box]: https://en.avm.de/products/fritzbox/
[MyFritz!]: https://en.avm.de/guide/myfritz-secure-access-to-your-data-anytime-anywhere/
[CloudflareWorkers]: https://www.cloudflare.com/learning/serverless/glossary/serverless-and-cloudflare-workers/
[DynDNS]: https://en.avm.de/service/knowledge-base/dok/FRITZ-Box-7590/1018_Determining-the-MyFRITZ-address-to-directly-access-FRITZ-Box-and-home-network-from-the-internet/
[encodeURI]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURI
[DynDNS-knowledge-base]: https://en.avm.de/service/knowledge-base/dok/FRITZ-Box-7590/30_Setting-up-dynamic-DNS-in-the-FRITZ-Box/

# Cloudflare Worker Fritz!Box DynDNS
A simple Cloudflare Worker script to update your IP address using the build-in Fritz!Box DynDNS.

> If you already have everything set up you can go directly to [Using the script/request URL](#using-the-scriptrequest-url).

----

## Why this script?
Why I did even wrote this script in the first place? There is already a service from [AVM] (the company behind [Fritz!Box]) called [MyFritz!] which does exactly that... Well, since I manage my domains with Cloudflare, I wanted to avoid an extra service where I need an extra account again. And since I already manage my domains via Cloudflare, I chose a Cloudflare Worker.

> <picture>
>   <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/light-theme/info.svg">
>   <img alt="Info" src="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/dark-theme/info.svg">
> </picture><br>
>
> If you already have a MyFritz! account from AVM and you are using this account actively, then you can use [their own DynDNS service][DynDNS], which is provided directly by AVM. 

----

## What even is a Cloudflare Worker?
A Cloudflare Worker is basically just a script that is stored under a certain URL and is executed when this URL is called. Or rather, that's what we use it for. Cloudflare Workers can do so much more. For more info, check out [this overview][CloudflareWorkers] to see what else you can use Cloudflare Workers for.

----

## Getting started
Okay, but now let's begin. In this guide I will explain how you can set up your own DynDNS service using a free Cloudflare Worker. I will go into all the steps that are necessary to create the Worker and how to set the service up correctly.

### Create Account/Login
To get started you need a Cloudflare Acoount (if you don't already have one). Go to [dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up) to sign up or sign in with an existing account at [dash.cloudflare.com/login](https://dash.cloudflare.com/login).

### Create first Worker
After you log in/register you should land on your dahboard. On the left hand side you should now see a menu point called "Workers". Click on this item. When you create a worker for the very first time you will land on the example page with the "Hello World sample worker" which looks something like this:

![Build your first Worker](https://user-images.githubusercontent.com/62719703/227031152-d47203c4-4011-4057-a4c7-c1887737fc2f.png)

Click on "Create Worker" below.

![Create Worker](https://user-images.githubusercontent.com/62719703/227032045-5cac4038-30c9-4581-8ae1-9364c6535043.png)

### Edit Worker
Then after that you will be taken directly to the "Quick Edit" view of the created Worker. On the left side you can see which code is executed inside the Worker. That means which code is responsible for our DynDNS service in the end. Now delete the code template example that was automatically added when you created it and replace it [with the code from this repository](./worker.log.js).

> <picture>
>   <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/light-theme/info.svg">
>   <img alt="Info" src="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/dark-theme/info.svg">
> </picture><br>
> 
> Optionally you can also use the [minified version](./worker.min.js) of the script.

After replacing the template with the code from the repository, click on the "Save and deploy" button at the bottom. If you have done everything right, you should be automatically returned to your account home page. Going again back to the left side in the menu under Workers, you should see your newly created Worker.

![Newly created Worker](https://user-images.githubusercontent.com/62719703/227037626-b7748d8f-902d-4c06-b336-28946f08cac3.png)

### Quick-Edit existing Worker
You can edit your worker at any time using the "Quick edit" button.

![Worker overview](https://user-images.githubusercontent.com/62719703/227038479-6cdafdb6-30d2-4e2c-8ca2-364058a83df4.png)

### Changing Sub-Domain for Workers
You can also change the sub-domain for all the Workers for your account. To do this, go back to the home page for all workers. Then on the right side you can change the sub-domain.

![Change subdomain](https://user-images.githubusercontent.com/62719703/227036851-1d33cc56-177e-452f-98e8-46aa88fabdfd.png)

----

## Get and configure Cloudflare API Token
Next, we'll look at how you can generate an API token for your Cloudflare account. To do this, log into your account and go to your profile, at [dash.cloudflare.com/profile](https://dash.cloudflare.com/profile).

![My Profile](https://user-images.githubusercontent.com/62719703/228295030-ffb1bdec-964a-4323-baf1-e013b8ea8ec3.png)

On the left side navigate to "API Tokens", under [dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens).

### Generate API Token

Now we will create a new API token. If you already have an API token from Cloudflare and it has the necessary permissions, then you can use this token.

Click on "Create Token"

![Create API Token](https://user-images.githubusercontent.com/62719703/228299225-87c3178d-52e2-4969-9749-1c87dba5cfe1.png)

Next, we will create a "Custom token" with the necessary permissions. To do this, click on Create Custom Token "Get started".

![Create custom token](https://user-images.githubusercontent.com/62719703/228302934-30c4a3c4-840d-4eb3-bd7e-e0127faba203.png)

Give the token a name and then set these permissions for the token. You can also add more permissions, the important thing is that the token can be used to access your zone or the gateway API in the end.

![Set permissions](https://user-images.githubusercontent.com/62719703/228305628-6fc62e5d-6722-42dd-b72a-4fbe5893864c.png)

Confirm the settings by clicking on "Continue to summary" below.

![Continue to summary](https://user-images.githubusercontent.com/62719703/228307119-a77f620a-2bc2-4a49-8b5b-df40e10e64ad.png)

Your summary should look similar to this. If so click on "Create Token"

![Create Token](https://user-images.githubusercontent.com/62719703/228308828-0e77522a-1128-4b9b-a4c1-ab262ef9154d.png)

Now you can copy your token and use it for the Worker.

![API-Token](https://user-images.githubusercontent.com/62719703/228312145-2fd97ea5-ffc8-444a-8bd6-888e9007bf7b.png)

----

## Using the script/request URL
Next, we'll look at how to properly use the Worker's request URL. We will also take a look at how to correctly enter the update URL into your Fritz!Box. 

### Basic URL structure
Let's look at the basic structure of the request URL. The URL is structured as follows:

| Worker subdomain | Account name | Cloudflare Worker domain |
|:----------------:|:------------:|:------------------------:|
|  `<subdomain>`   |   `<name>`   |      `workers.dev/`      |

> <picture>
>   <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/light-theme/example.svg"/>
>   <img alt="Info" src="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/dark-theme/example.svg"/>
> </picture><br>
>
> An example would look like this: `https://random-name.your-account.workers.dev/`

### URL parameters

The following parameters can be used in the URL:

|   Parameter   | Datatype |   Required   | Default Value | Description                                                     |
|:-------------:|:--------:|:------------:|---------------|-----------------------------------------------------------------|
|    `token`    |  string  |      yes     | ""            | Token for the Cloudflare API                                    |
|    `zoneid`   |  string  |      yes     | ""            | ID for the DNS Zone                                             |
| `ipv4address` |  string  |      yes     | ""            | IPv4 address to update                                          |
|   `ipv4name`  |  string  |      yes     | ""            | IPv4 domain name                                                |
| `ipv4proxied` |  boolean |      no      | true          | If the IPv4 connection should be proxied                        |
|   `ipv4ttl`   |  number  |      no      | 1             | IPv4 Time to live (1 = Auto, 60-86400 = Valid range)            |
| `ipv6address` |  string  |      no      | ""            | IPv6 address to update                                          |
|   `ipv6name`  |  string  | (yes)[*][*1] | ""            | IPv6 domain name (*Only required if `ipv6address` is specified) |
| `ipv6proxied` |  boolean |      no      | true          | If the IPv6 connection should be prodied                        |
|   `ipv6ttl`   |  number  |      no      | 1             | IPv6 Time to live (1 = Auto, 60-86400 = Valid range)            |
|   `comment`   |  string  |      no      | ""            | Comment for the created/updated record                          |

[*1]: README.md## "*Only required if 'ipv6address' is specified"

Parameters are simply appended to the request URL with a `?`. Between the parameters are `&` characters. For more information, please reference to [this article from MDN][encodeURI] on how to properly encode URL parameters.

### Placeholders
Furthermore, there are a few placeholders which are automatically replaced by the Fritz!Box:

|      Parameter       | Description                                                                                                       |
|:--------------------:|:------------------------------------------------------------------------------------------------------------------|
|     `<username>`     | Username                                                                                                          |
| `<pass>`/`<passwd>`  | Password (Token)                                                                                                  |
|      `<domain>`      | Domain                                                                                                            |
|      `<ipaddr>`      | IPv4 address of the Fritz!Box                                                                                     |
|     `<ip6addr>`      | IPv6 address of the Fritz!Box                                                                                     |
|   `<ip6lanprefix>`   | IPv6 prefix for home network                                                                                      |
|    `<dualstack>`     | Dual-stack                                                                                                        |
| `<useragent>`[*][*2] | Device that sends the request (*This is not directly from Fritz!Box, <br>it is implemented via the Worker script) |

[*2]: README.md## "*This is not directly from Fritz!Box,&#013;it is implemented via the Worker script"

For more information please reference to [this knowledge base][DynDNS-knowledge-base] from AVM on "2 Setting up dynamic DNS".

> <picture>
>   <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/light-theme/example.svg"/>
>   <img alt="Info" src="https://github.com/Mqxx/GitHub-Markdown/blob/main/blockquotes/badge/dark-theme/example.svg"/>
> </picture><br>
>
> An example with the parameters would look like this:<br>
> `https://random-name.your-account.workers.dev?token=abc1234&zoneid=1234&ipv4address=<ipaddr>&ipv4name=example.com&comment=This%20is%20a%20comment`<br>
>
> Or with more placeholders:<br>
> `https://random-name.your-account.workers.dev?token=<passwd>&zoneid=<username>&ipv4address=<ipaddr>&ipv4name=<domain>&comment=This%20is%20a%20comment%20from:<useragent>`<br>
> Now you can put your credentials inside the password and username field.

<br>
<br>
<br>
