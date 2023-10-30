# Vue2 vs Vue3

Vue 3 has changed its reactivity system from being based on Object.defineProperty's getter/setter to using Proxy, which brings many differences.

For large objects, in the Vue 2 era, the cost of reactive object creation was mainly concentrated during creation. Once getters/setters were created, access was quite fast. However, in Vue 3, creating a reactive object using Proxy is very fast, but accessing the Proxy object takes longer compared to getter/setter.


For our complex web application, modifying large reactive objects is crucial. This test is designed to compare the actual performance of Vue 3 and Vue 2 in this specific aspect.

## Test Result



