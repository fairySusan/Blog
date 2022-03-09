Hook使用了javascript的闭包机制
`useEffect`让你在函数组件中执行副作用：数据获取、设置订阅、手动更改react的Dom
副作用分为需要清除的、不需要清除的
无需清除的：网络请求、手动变更DOM、记录日志
需要清除的：订阅

1、effect会在每次渲染的的时候都执行
2、react会在组件卸载的时候执行清除操作
3、React会在执行当前的effect的时候清除上一个effect

useEffect 不给第二个参数：挂载的时候、每次视图更新后都会调用
useEffect 给一个空数组：只会在挂载的时候调用
useEffect 给一个非空数组：挂载的时候和订阅的state更新的时候
组件卸载的时候调用useEffect返回的函数，清除副作用
