---
title: 探究useEffect，useMemo, useCallback
linktitle: 探究useEffect，useMemo, useCallback
type: book
date: "2021-08-12T07:20:00+08:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---
这周末花时间系统地总结了一下React中最常用的两个hooks，useEffect和useMemo/useCallback。深入阅读了几篇博客的文章并且结合自己的试验，写了这篇报告：
 
### 关于useEffect
1. Batch update https://medium.com/swlh/react-state-batch-update-b1b61bd28cd2
有一个batch update的机制，不是每一次setState都之后都重新渲染，是成批更新state
componentDidMount，useEffect和event Handler都是batch update
在promise中的更新不是成批的
 
2. dependency list https://dmitripavlutin.com/react-useeffect-infinite-loop/
为了阻止无限循环，要给useEffect传入dependency list
避免使用object作dependency，因为每次重新渲染会生成不同的object
默认的dependency是组件的所有props和states
空的dependency list表示useEffect只在初次渲染之后运行一次
 
3. return value https://reactjs.org/docs/hooks-effect.html
return value是useEffect的cleanup机制，后面接返回时要执行的函数，相当于componentWillUnMount
写了一个每一秒钟print一个hello world的程序：https://codesandbox.io/s/vibrant-goldberg-qhqt5
 
关于React memo https://dmitripavlutin.com/use-react-memo-wisely/
1. React组件会在state或props发生改变时自动重新渲染，在一些情况下会造成unnecessary 渲染s。比如，如果Parent组件包含Child组件，Parent的一个prop经常变化但是Child的props不变，那么每次都要重新渲染 Child组件。为了提升性能，可以给Child组件包裹一层React.memo，这样，在下一次渲染之前，React会把Child的props 和前一次渲染比较，若props没有改变，则直接跳过下一次渲染，例子：https://codesandbox.io/s/holy-brook-0r3pd?file=/src/App.js:507-565
2. 如果是function做props还要把function给用useCallback包裹起来，这是useCallback的用途之一，例子同上：https://codesandbox.io/s/holy-brook-0r3pd?file=/src/App.js:507-565
3. 什么样的组件适合用React.memo？
组件比较大，经常渲染，经常传入同样的props
4. 什么样的组件不适合用React.memo？
       Props总是在变化时不适用。因为Parent props更新时，React会做两件事：
第一，  用comparison function比较prevProps和nextProps
第二，  由于comparison function总是放返回false，React会运行diff算法
造成了计算资源损耗
5. 此外，React memo的第二个参数可以用来自定义props的比较函数
 
### 关于useMemo/useCallback
1. 首先读了下面这两篇文章，回顾了一下useMemo和useCallback的用法
如何使用useMemo https://www.educative.io/edpresso/when-should-you-use-usememo-in-react
如何使用useCallback https://dmitripavlutin.com/dont-overuse-react-usecallback/
总结起来用法不外乎以下两条
       useMemo(() => heavyCalc(a, b), [a, b])
       useCallback(callback, [prop]);  useMemo(() => callback, [prop]);
 
2. 然后，另外一个关键问题是为什么不要在项目中滥用useCallback和useMemo，下面这两篇文章做了详细的讲解：
https://dmitripavlutin.com/dont-overuse-react-usecallback/
https://kentcdodds.com/blog/usememo-and-usecallback
 
其中，Kent C. Dodds的文章指出以下代码
const dispense = useCallback(candy => {
       setCandies(allCandies => allCandies.filter(c => c !== candy))
})
相当于
const dispense = candy => {
       setCandies(allCandies => allCandies.filter(c => c !== candy))
}
+ const dispenseCallback = React.useCallback(dispense);
 
也就是说，重新渲染的时候，useCallback里面的function不仅会被重新定义一次，而且之前定义的函数也不会被回收，还要多运行一次React.useCallback。因此，如果在一般情况下使用useCallback会导致网页性能更差。
 
### 那么，什么时候要使用useMemo和useCallback呢?
1. 前面提到了，React.memo可以防止unnecessary 渲染，用它包裹的组件（通常是很大的组件）的props经常会有 functions，因此用到useCallback: https://codesandbox.io/s/young-pond-g7b7h?file=/src/App.js
2. Dependency list里面的参数有时候是objects/arrays/functions，这时需要用useMemo和useCallback，例如https://codesandbox.io/s/angry-shockley-h0iub?file=/src/App.js
3. 此外，useMemo还有一个用途是useCallback 不具备的，用它把computationally expensive的计算结果保存下来，避免重复计算：https://codesandbox.io/s/adoring-montalcini-6i972
