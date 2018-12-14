<img src="pyyuque.png" height="100px" height="50px" /> 

# Simple PyYuQue

[![Build Status](https://travis-ci.org/Xarrow/simple-pyyuque.svg?branch=master)](https://travis-ci.org/Xarrow/simple-pyyuque)
[![codecov](https://codecov.io/gh/Xarrow/simple-pyyuque/branch/master/graph/badge.svg)](https://codecov.io/gh/Xarrow/simple-pyyuque)

一个非官方的 [“语雀”](http://yuque.com) 的Python API 封装。提供和官方 API 类似的调用方式。设计简单，运行高效。

详细文档说明参考[https://www.yuque.com/yuque/developer/api](https://www.yuque.com/yuque/developer/api)

----
# 安装
TODO

----
# 快速开始

### 1. 实例化

```python
spyq = SimplePyYuQueAPI(token="token", app_name="py_yuque")

```
其中 `token` 是在语雀中 setting -> token 中申请 ， `app_name` 为你的应用名称。

### 2. 简单调用

* 获取认证的用户的个人信息

```python

spyq = SimplePyYuQueAPI(token="token", app_name="py_yuque")
u = spyq.User()
user_serializer = u.get_user()

```

可以更加简洁一点

```python
user_serializer = SimplePyYuQueAPI(token="token", app_name="py_yuque1").User().get_user()

```

你还可以打印出原始报文
```python
print(user_serializer.base_response)
```

* 基于用户 login 或 id 获取一个用户的基本信息。
```python
user = SimplePyYuQueAPI(token="token", app_name="py_yuque").User()
print("==> Helixcs is %s", user.get_users(login="Helixcs").base_response)

# ==> Helixcs is {'id': 104023, 'type': 'User', 'space_id': 0, 'account_id': 10838, 'login': 'helixcs', 'name': 'Helixcs', 'avatar_url': 'https://cdn.nlark.com/yuque/0/2018/png/104023/1539315567419-aad17f80-8365-4a08-af1e-e301a3c2c7f5.png', 'large_avatar_url': 'https://cdn.nlark.com/yuque/0/2018/png/104023/1539315567419-aad17f80-8365-4a08-af1e-e301a3c2c7f5.png?x-oss-process=image/resize,m_fill,w_320,h_320', 'medium_avatar_url': 'https://cdn.nlark.com/yuque/0/2018/png/104023/1539315567419-aad17f80-8365-4a08-af1e-e301a3c2c7f5.png?x-oss-process=image/resize,m_fill,w_160,h_160', 'small_avatar_url': 'https://cdn.nlark.com/yuque/0/2018/png/104023/1539315567419-aad17f80-8365-4a08-af1e-e301a3c2c7f5.png?x-oss-process=image/resize,m_fill,w_80,h_80', 'books_count': 3, 'public_books_count': 2, 'followers_count': 3, 'following_count': 3, 'public': 1, 'description': 'Java 糊口，Python 兴趣', 'created_at': '2018-04-23T02:43:33.000Z', 'updated_at': '2018-12-07T17:00:03.000Z', '_serializer': 'v2.user_detail'}

```

详细API参考官方文档：[https://www.yuque.com/yuque/developer/user](https://www.yuque.com/yuque/developer/user)

----
# API 说明

### 1. User 用户

* 获取认证的用户的个人信息

```python
 user_api = SimplePyYuQueAPI(token="token", app_name="py_yuque1").User()
 print(user_api.get_user())
 print(user_api.user)
```

返回: `UserSerializer`

* 获取单个用户信息

```python

user.get_users(login="Helixcs")

user.get_users(id=104023)

```

返回: `UserSerializer`


* 获取我创建的文档

```python
user.get_user_docs()
user_api.get_user_docs(q='',offset=1)

```

返回: `Array<DocSerializer>`

* 获取我最近参与的文档/知识库

```python
user.get_user_recent_updated()
user.get_user_recent_updated(type=UserDescriptionType.BOOK)
user.get_user_recent_updated(type="Doc")

```

返回: `Array<DocSerializer>` 或 `Array<BookSerializer>`


### 2. Group 组织

* 获取某个用户的加入的组织列表

```python
 group_api = SimplePyYuQueAPI(token="token", app_name="py_yuque1").User()
 group_api.get_users_groups(login="Helixcs")

```

返回：`Array<UserSerializer>`

* 获取公开组织列表

```python
group_api.get_public_groups()
group_api.public_groups

```

返回：`Array<UserSerializer>`

* 创建 Group

```python

group_api.post_group(name="Helixcs 的组织名称", login="Helixcs123",description="Helixcs 的组织描述")

```

返回：UserSerializer

访问：`https://www.yuque.com/<login>` 查看新建 Group。


* 获取单个组织的详细信息

```python
group_api.get_groups_detail(id=225250)
group_api.get_groups_detail(login="Helixcs123")

# https://www.yuque.com/helixcs123

```
返回：UserSerializer

* 更新单个组织的详细信息

```python
group_api.put_groups(login="Helixcs123",name="Helixcs 的组织名称更新1次",login_update="Helixcs456",description="Helixcs123 更新为Helixcs456")
group_api.update_groups(login="Helixcs123",name="Helixcs 的组织名称更新2次",login_update="Helixcs123",description="Helixcs123 更新为Helixcs456")

# https://www.yuque.com/helixcs123
```

返回：UserSerializer

访问：`https://www.yuque.com/<login>`


* 删除组织
```python
group_api.delete_groups(login="Helixcs456")
group_api.delete_groups(id=225250)

```

返回：UserSerializer

* 获取组织成员信息

```python
# 这里的 login 为 group name
group_api.get_groups_users(login="Helixcs456")

# 这里的 id 为 group_id
group_api.get_groups_users(id=225250)

```

返回：Array<GroupUserSerializer>


* 增加或更新组织成员

```python
group_api.put_groups_users(group_login="Helixcs456",
                                         login="OtherUser",
                                         role=1)

group_api.update_group_users(group_login="Helixcs456",
                                         login="OtherUser",
                                         role=1)
```

返回：GroupUserSerializer

* 删除组织成员
```python
group_api.delete_groups_users(group_login="Helixcs456",
                              login="OtherUser")

group_api.delete_groups_users(group_id=225250,
                              login="OtherUser")
```

返回：GroupUserSerializer

### 3. Repo 资源

* 获取某个用户/组织的仓库列表

```python
repo_api.get_users_repos(type="all",login="Helixcs")

```

返回：Array<BookSerializer>

TODO

### 4. Doc 资源

TODO

----
# 问题排查

TODO：

----
# LICENSE

MIT


