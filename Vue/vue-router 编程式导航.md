# vue-router 编程式导航

## this.$router.push

- 传入字符串 --> /home

```
this.$router.push('home')
```

- 传入对象 --> /home

```
this.$router.push({ path: '/home' })
```

- 传入带路由名的对象 --> /user/123

```
this.$router.push({ name: 'user', params: { userId: '123' }})
```

- **注意：如果提供了 path，params 会被忽略** --> /user

```
this.$router.push({ path: '/user', params: { userId: '123' }})
```

- 传入可变路径 --> /user/123

```
this.$router.push({ path: `/user/${userId}` })
```

- 带查询参数 --> /user?userId=123

```
this.$router.push({ path: `/user`, query: { userId: '123' } })
```

## this.$router.replace

跟 `router.push` 很像，唯一的不同就是，它不会向 history 添加新记录，而是替换掉当前的 history 记录。

## this.$router.go

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)。

```
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)
```