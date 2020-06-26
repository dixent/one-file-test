---
title: Async Data
description: You may want to fetch data and render it on the server-side. Nuxt.js adds an `asyncData` method to let you handle async operations before setting the component data.
---

> You may want to fetch data and render it on the server-side. Nuxt.js adds an `asyncData` method to let you handle async operations before initializing the component

## The asyncData method

Sometimes you just want to fetch data and pre-render it on the server without using a store.
`asyncData` is called every time before loading the **page** component.
It will be called server-side once (on the first request to the Nuxt app) and client-side when navigating to further routes.
This method receives [the context](/api/context) as the first argument, you can use it to fetch some data and Nuxt.js will merge it with the component data.

Nuxt.js will automatically merge the returned object with the component data.
