# Vue2 vs Vue3

Vue 3 has changed its reactivity system from being based on Object.defineProperty's getter/setter to using Proxy, which brings many differences.

For large objects, in the Vue 2 era, the cost of reactive object creation was mainly concentrated during creation. Once getters/setters were created, access was quite fast. However, in Vue 3, creating a reactive object using Proxy is very fast, but accessing the Proxy object takes longer compared to getter/setter.


For our complex web application, modifying large reactive objects is crucial. This test is designed to compare the actual performance of Vue 3 and Vue 2 in this specific aspect.



- [vue2-test]([./](https://yarna.github.io/Vue2-vs-Vue3/vue2/dist/index.html))
- [vue3-test]([./](https://yarna.github.io/Vue2-vs-Vue3/vue3/dist/index.html))

## Test Result

Based on the results, the overhead of modifying Vue 3 reactive objects seems to be 2 to 3 times higher compared to Vue 2. However, the absolute value of this overhead is small, making it challenging to notice in daily scenarios with small data sets. Nevertheless, the cumulative effect becomes apparent when repeatedly modifying large objects, revealing the difference in performance.


Vue3 has likely made significant improvements in component rendering, which compensates for the speed disadvantage of its reactivity system compared to Vue2. In most cases, the differences in the end results when using Vue2 or Vue3 aren't substantial.

Vue3 has likely made significant improvements in component rendering, which compensates for the speed disadvantage of its reactivity system compared to Vue2. In most cases, due to the proportionally smaller time spent on modifying reactive objects compared to component rendering, Vue3 is generally faster overall.

However, it's essential to note that when the modifications to reactive data become more frequent, taking up a larger proportion of the total execution time compared to component rendering, Vue3's overall speed might not surpass Vue2.

<img width="1232" alt="image" src="https://github.com/yArna/Vue2-vs-Vue3/assets/82231420/9ccc9deb-07cb-48dd-9ab1-28d52a5d0689">

_ |Vue2|Vue3
---|---|---
Init Data|166|0.1
Mounted|2418|2541
Selected(Js)|29|137
Selected(NextTick)|1569|1020
ChangeData(Js)|65|222
ChangeData(NextTick)|2365|2245
ChangeSort(Js)|49|234
ChangeSort(NextTick)|3564|3603

