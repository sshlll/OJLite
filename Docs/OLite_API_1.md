# OJLite API拟定稿（版本1）

**API调用格式：`/api.php?api={api_id}&...`**

*API1*:获取用户信息

输入：

```javascript
type:检索的关键字，可为'uid'或'uname'
data:关键字的值
```

输出：

```javascript
[JSON]用户信息(status,uid,uname,priv,rank,saying,email)
status:{200：OK}，{404：用户不存在}
uid:用户编号
uname:用户昵称
priv:权限(0:封禁，1：普通用户，2：管理员，3：超级管理员)
rank:用户贡献值
saying:用户留言
email:用户邮件地址
```



*API2*:用户密码验证

输入：

```javascript
type:验证的关键字，可为'uid'或'uname'
data:关键字的值
passwd:密码
```

输出：

```javascript
[JSON]用户信息(status,uid,uname,salt)
status:{200：OK}，{404：用户不存在}，{503：密码错误}
salt：用户验证代码({uid:用户uid，passwd:用户密码})
```



*API3*:获取题目信息

输入：

```javascript
type:检索的关键字，可为'pid'或'pname'
data:关键字的值
```

输出：

```javascript
[JSON]题目信息(status,pid,pname,detail,t_limit,m_limit,testcount)
status:{200：OK}，{404：题目不存在}
pid:题目编号
pname:题目名称
detail:题目描述
t_limit：时间限制
m_limit:空间限制
testcount:测试点数
```



*API4*:获取题目统计信息

输入：

```javascript
pid:关键字的值
```

输出：

```javascript
[JSON]题目统计信息(status,count,fastest_id,shortest_id,[JSON_ARRAY]([JSON]单条提交记录(rid,uid,score,size,time,memory,code)))
status:{200：OK}，{404：题目不存在}
count:提交记录条数
fastest_id:最快编号
shortest_id:最短编号
rid：评测编号
uid：提交用户编号
score：评测分数
time：评测耗时
memory：评测内存消耗
code：提交的代码
```



*API5*:提交代码以评测

输入：

```php+HTML
salt：用户验证代码({uid:用户uid，passwd:用户密码})
pid:题目编号
code：提交的代码
lang:程序语言
optimization:优化等级
```

输出：

```javascript
[JSON](status,(rid),rank)
status:{200：OK}，{404：题目不存在}，{503：用户验证错误}，{504：贡献值不足}
rank:用户贡献值
if status == 200:
	rid:评测编号
```



API6:拉取服务器代码以在本地评测

输入：

```php+HTML
salt：用户验证代码({uid:用户uid，passwd:用户密码})
```

输出：

```javascript
[JSON](status,rid,code,t_limit,m_limit,count,[JSON_ARRAY]([JSON]输入输出文件[in_file,out_file]))
status:{200：OK}，{404：无待评测程序}，{503：用户验证错误}
rid：评测编号
code：提交的代码
t_limit：时间限制
m_limit:空间限制
count:数据组数
in_file:输入数据
out_file:答案数据
```



API7:向服务器提交本地评测结果

输入：

```php+HTML
salt:用户验证代码({uid:用户uid，passwd:用户密码})
rid：评测编号
score:成绩
code:提交的代码
compile_info:编译信息
count:数据组数
pts_info:[JSON_ARRAY]([JSON](type:当前数据点结果(AC,WA,...,etc.),cur_score:当前数据点得分,output:程序输出))
```

输出：

```javascript
[JSON](status,rank)
status:{200：OK}，{503：用户验证错误}
rank:用户贡献值
```



*API8*:用户注册

输入：

```javascript
uname:用户昵称
passwd:密码
saying:用户留言
email:用户邮件地址
```

输出：

```javascript
[JSON]用户信息(status,uid)
status:{200：OK}，{503：服务器拒绝}
uid:注册的用户编号
```



*API9*:用户管理（仅管理员）

输入：

```javascript
salt:用户验证代码({uid:用户uid，passwd:用户密码})
uid:用户编号
modify:[JSON_ARRAY]要修改的信息
```

输出：

```javascript
[JSON]用户信息(status)
status:{200：OK}，{503：权限不足},{404:用户不存在}
```



*API10*:题目上传

输入：

```javascript
salt:用户验证代码({uid:用户uid，passwd:用户密码})
pname:题目名称
detail:题目描述
t_limit：时间限制
m_limit:空间限制
testcount:测试点数
data:[JSON_ARRAY]([JSON]输入输出文件[in_file,out_file])
```

输出：

```javascript
[JSON]题目信息(status,pid)
status:{200：OK}，{503：权限不足}
pid:题目编号

```

