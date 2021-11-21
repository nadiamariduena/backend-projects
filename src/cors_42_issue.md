## Access-Control-Allow-Origin is a CORS (Cross-Origin Resource Sharing) header.

<br>

#### [how-does-access-control-allow-origin-header-work](https://stackoverflow.com/questions/10636611/how-does-access-control-allow-origin-header-work)

<br>

> When Site A tries to fetch content from Site B, Site B can send an Access-Control-Allow-Origin response header to tell the browser that the content of this page is accessible to certain origins. (An origin is a domain, plus a scheme and port number.) By default, Site B's pages are not accessible to any other origin; using the Access-Control-Allow-Origin header opens a door for cross-origin access by specific requesting origins.

- For each resource/page that Site B wants to make accessible to Site A, Site B should serve its pages with the response header:

```javascript
Access-Control-Allow-Origin: http://siteA.com
```

### Enabling CORS

##### [lets-talk-about-cors](https://medium.com/bigcommerce-developer-blog/lets-talk-about-cors-84800c726919)

> check it in incognito if its blocked by medium

- Let’s consider scenario one: you control the server for the remote resource that you’re requesting. That’s good news! That means that you can enable CORS on your server and access the resources you need from any origins you grant permission to.

<br>

#### What to Do When Enabling CORS Is Not an Option

> Now for scenario two: you want to make a request from the browser to a resource on a server that you don’t control; for example, a public API that isn’t CORS-enabled. This is a pretty common scenario when calling a public API from your client-side app. What options do you have to make a cross origin request when you can’t enable CORS?

#### Use a proxy

Keep in mind that the same-origin policy only comes into play only when making a request from the browser using JavaScript. You can request resources from different origins all day long — as long as you’re making that request from a server. A proxy is a layer that sits between your request and its destination, obscuring the request’s origin. One solution for making cross-origin requests is to use a CORS proxy to make it seem as though you’re making the request from a location that’s allowed.

<br>

There are multiple CORS proxies out there that you can use for free. Two of the most well known are cors-anywhere and crossorigin.me. Crossorigin.me is easy to use — you simply prepend https://crossorigin.me/ to your request URL — but it’s not a tool that you want to use in production. Free proxies can be great for testing, but you wouldn’t want to rely on a free third party tool for something that you’ll actually be using in production. In those cases, you’ll want a more stable solution to call the API from a server and make the data available client-side.

<br>

#### Use a serverless function

A serverless function is another way to proxy your requests, but instead of relying on a free third-party service of unknown reliability, you’re building your own micro-infrastructure to call a web service and feed data to an API endpoint. Popular options for serverless functions include GCP or AWS, and they allow you to stake out just enough server space to run a function or two. That’s all we need in this case, because we really just need to do one thing: run a function that calls a webservice to return some data.
Let’s imagine that we’ve written a function to call the BigCommerce catalog API, and we plan to run it as an AWS serverless function. This solves 2 problems: 1) We don’t have to worry about exposing API credentials, because they’re safely stored server-side, and 2) We don’t have to worry about CORS errors when making the request to the BigCommerce API because again, server-side.

<br>

We do still need to get that data to our client-side JavaScript, however. The way we can do this is by configuring an Amazon API Gateway endpoint that acts as a trigger. When our client-side JavaScript sends an HTTP request to the API gateway endpoint, we want it to run the function and send back the response from the BigCommerce catalog API. But, now we have to think about CORS again, because the request from our JavaScript to the Amazon API Gateway endpoint goes from the browser to — you guessed it — a different origin. But, in this scenario, we control the server that’s receiving the request. We can configure our Amazon API Gateway with CORS headers that say it accepts requests from all domains (\*) or just the domain on which we’re running the client-side script.

### [credits: lets-talk-about-cors](https://medium.com/bigcommerce-developer-blog/lets-talk-about-cors-84800c726919)

<br>
<hr>
