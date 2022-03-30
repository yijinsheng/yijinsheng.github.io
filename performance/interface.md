# 绩效管理接口文档
# 返回码描述
|  返回码  |  描述 |
|  ----  | ----  |
| 0  | system.success|
| -1  | 系统内部错误 |
|1 | 请求失败 |
| 11000  | 手机号不存在 |
| 11003  | 验证码未失效 |
|11004 | 短信发送故障 |
|11005 | 绩效计划尚未审核完成 |
|11006 | 已经完成绩效自评 |
|11007 | 已经完成对该员工绩效评定 |


## context_path: 
/performance/plat
## 短信验证码获取接口获取
### url:
/public/sms/send
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |

### body:
|  参数  |  描述 |
|  ----  | ----  |
| mobile  | 手机号 |
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  | 返回数据 |

## 登录接口
### url:
/public/login/bySms
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |

### body:
|  参数  |  描述 |
|  ----  | ----  |
| mobile  | 手机号 |
| vCode  | 手机号 |
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{token:登录认证token，userCode:用户编码} |

## 角色列表
### url:
/role/list
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{roleName:角色名，id:角色id} |
### 返回例子
```
{"code":0,"msg":"system.success","data":[{"id":1,"roleName":"员工"}, {"id":2,"roleName":"团队总监"}, {"id":3,"roleName":"部门总监"}, {"id":4,"roleName":"副总"}, {"id":5,"roleName":"综合管理"}]}
```

## 部门列表
### url:
/department/list
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |

### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{departmentName:部门名，id:部门id} |
```
{"code":0,"msg":"system.success","data":[{"id":1,"departmentName":"连接业务部"}, {"id":2,"departmentName":"应用与部件业务部"}, {"id":3,"departmentName":"综合服务支撑中心"}, {"id":4,"departmentName":"可信供应链BU"}, {"id":5,"departmentName":"物联创新中心"}, {"id":6,"departmentName":"平台研发中心"}, {"id":7,"departmentName":"经营管理部"}, {"id":8,"departmentName":"工业互联网BU"}, {"id":9,"departmentName":"城市物联网感知BU"}, {"id":10,"departmentName":"销售支持中心"}]}
```

## 根据部门id查询团队列表
### url:
/team/findByDepartmentId
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |

### body

|  参数  |  描述 |
|  ----  | ----  |
| departmentId  | 部门id|
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{teamName:部门名，departId:角色id，id:团队id} |
```
{"code":0,"msg":"system.success","data":[{"id":1,"teamName":"团队A","departId":1}, {"id":2,"teamName":"团队B","departId":1}]}
```

## 添加用户
### url:
/user/addUser
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |

### body
|  参数  |  描述 |
|  ----  | ----  |
| mobile| 手机号|
| roleId| 角色编号|
|realName|姓名|
|departId|部门Id|
|teamId|团队id|
|email|邮箱地址|
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||


## 检查登录状态

### url:
/public/login/checkLoginState
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |

### body
|  参数  |  描述 |
|  ----  | ----  |
| mobile| 手机号|
| token| 登录token|

### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||

## 绩效计划保存草稿

### url:
/performancePlan/save
### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
| performancePlanDetails| List\<PerformancePlanDetail\>|
#### PerformancePlanDetail
|  参数  |  描述 |
|  ----  | ----  |
| kpi| kpi|
| weight| 权重|
| indicatorDescription| 指标说明|
| yearGoal| 年度目标|
| goalA| 第一季度目标|
| goalB| 第二季度目标|
| goalC| 第三季度目标|
| goalD| 第四季度目标|
| sortIndex| 用作排序显示|
### 请求例子
```
{
  "performancePlanDetails": [
    {
      "kpi": "kkkkkk",
      "weight": 0.35,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 1
    },
    {
      "kpi": "kkkkkk",
      "weight": 0.35,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 2
    },
    {
      "kpi": "kkkkkk",
      "weight": 0.15,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 3
    },
    {
      "kpi": "kkkkkk",
      "weight": 0.15,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 4
    }
  ]
}
```
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||

## 绩效计划提交

### url:
/performancePlan/create

### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
| performancePlanDetails| [PerformancePlanDetail]|
#### PerformancePlanDetail
|  参数  |  描述 |
|  ----  | ----  |
| kpi| kpi|
| weight| 权重|
| indicatorDescription| 指标说明|
| yearGoal| 年度目标|
| goalA| 第一季度目标|
| goalB| 第二季度目标|
| goalC| 第三季度目标|
| goalD| 第四季度目标|
| sortIndex| 用作排序显示|
### 请求例子
```
{
  "performancePlanDetails": [
    {
      "kpi": "kkkkkk",
      "weight": 0.35,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 1
    },
    {
      "kpi": "kkkkkk",
      "weight": 0.35,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 2
    },
    {
      "kpi": "kkkkkk",
      "weight": 0.15,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 3
    },
    {
      "kpi": "kkkkkk",
      "weight": 0.15,
      "indicatorDescription": "指标说明",
      "yearGoal": "年度目标",
      "goalA": "第一季度目标",
      "goalB": "第二季度目标",
      "goalC": "第三季度目标",
      "goalD": "第四季度目标",
      "sortIndex": 4
    }
  ]
}
```
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||

## 查看自己的绩效详情
### url:
/performancePlan/queryDetail

### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<PerformancePlanDetail\>|

### 查看自己绩效状态
### url:
/performancePlan/queryPlan

### method
post
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### 返回 
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |PerformancePlan|
#### PerformancePlan
|  参数  |  描述 |
|  ----  | ----  |
| id  | 计划绩效id（planId）|
| state  | 流程状态，0-保存 ，1-团队总监审核中，2-部门总监审核中，3-副总审核中，4，完成状态，状态为0时候agreementTeam=2为团队总监驳回，agreementDepart=2 为部门总监驳回，agreementVice=2为副总驳回|
| year  | 年份|
| userId  | userId|
| roleId  | roleId|
| departId  | 部门Id|
| team_id  | 团队Id|
| realName  | 员工姓名|
| position  | 岗位|
| memoTeam  | 团队领导审批备注|
| memoDepart  | 部门领导审批备注|
| memoVice  | 副总审批备注|
| isApprove  | 审批状态 0-未提交 1-未审批 2-团队总监已审批 3-部门总监已经审批 4-副总已经审批|
| agreementTeam  | 团队总监审批意见 1-通过 2-驳回|
| agreementDepart  | 部门总监审批意见 1-通过 2-驳回|
| agreementVice  |副总审批意见 1-通过 2-驳回|




## 负责人查看下属绩效计划详情
### url:
/performancePlan/queryUserDetail
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
| planId  | 绩效计划Id|

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<PerformancePlanDetail\>|

## 负责人查看下属绩效计划列表
### url:
/performancePlan/queryMemberPlan
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{"members":List\<PerformancePlan\>,"teamLeaders":List\<PerformancePlan\>}|

如果当前登录角色是团队总监，返回结果只有members，如果是部门总监则可以看到teamleaders和members，如果是副总，只会返回teamLeaders

## 负责人审批下属绩效计划详情
### url:
/performancePlan/approve
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
| planId  | 绩效计划Id|
| agreement  | 审批意见1-同意，2-驳回|
| memo  |审批意见|
### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |


## 持续绩效添加
### url:
/performancePlanCt/insert
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
| performanceCts  | List\<PerformanceCt\>|
### PerformanceCt
|  参数  |  描述 |
|  ----  | ----  |
| createTime  |yyyyMMdd|
| achievement  |成就活动名称|
| description  |详细描述|
| effect  |成效|
### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |


## 持续绩效获取员工列表
### url:
/performancePlanCt/insert
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{"members":List\<User\>,"leaders":List\<User\>}|
团队总监能看到所有的团队成员放在members里面
部门总监能看到所有团队成员(members)和所有团队总监（leaders）
副总能看到所有的团队总监（leaders）


## 查看员工持续绩效
### url:
/performancePlanCt/getUserInfo
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
| userId | userId (持续绩效获取员工列表f返回的id)|


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<PerformanceCt\>|


## 获取菜单接口
### url:
/public/menu/list
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<Menu\>|

#### menu
|  参数  |  描述 |
|  ----  | ----  |
| menuLevel  | 1：一级菜单 2:二级菜单|
| menuName  |菜单名称 |
| pMenuId  | 父菜单id|

| uri  | 资源路径|
### 返回结果例子
```
{
    "code": 0,
    "msg": "system.success",
    "data": [
        {
            "id": 1,
            "menuLevel": 1,
            "menuName": "首页",
            "uri": "/DASHBOARD",
            "cate": "DASHBOARD",
            "subMenuList": []
        },
        {
            "id": 2,
            "menuLevel": 1,
            "menuName": "绩效计划",
            "cate": "PL",
            "subMenuList": [
                {
                    "id": 7,
                    "menuLevel": 2,
                    "menuName": "计划设定",
                    "pMenuId": 2,
                    "uri": "/PL-SET"
                },
                {
                    "id": 8,
                    "menuLevel": 2,
                    "menuName": "计划调整",
                    "pMenuId": 2,
                    "uri": "/PL-ADJUST"
                }
            ]
        },
        {
            "id": 3,
            "menuLevel": 1,
            "menuName": "持续绩效",
            "cate": "PE",
            "subMenuList": [
                {
                    "id": 9,
                    "menuLevel": 2,
                    "menuName": "持续绩效添加",
                    "pMenuId": 3,
                    "uri": "/PE-ADD"
                },
                {
                    "id": 10,
                    "menuLevel": 2,
                    "menuName": "下属绩效成就",
                    "pMenuId": 3,
                    "uri": "/PE-SUB"
                }
            ]
        },
        {
            "id": 4,
            "menuLevel": 1,
            "menuName": "流程审批",
            "cate": "FL",
            "subMenuList": [
                {
                    "id": 11,
                    "menuLevel": 2,
                    "menuName": "目标设定审批",
                    "pMenuId": 4,
                    "uri": "/FL-SET"
                },
                {
                    "id": 12,
                    "menuLevel": 2,
                    "menuName": "过程修订审批",
                    "pMenuId": 4,
                    "uri": "/FL-EDIT"
                }
            ]
        },
        {
            "id": 5,
            "menuLevel": 1,
            "menuName": "绩效考核",
            "cate": "EX",
            "subMenuList": [
                {
                    "id": 13,
                    "menuLevel": 2,
                    "menuName": "员工自评",
                    "pMenuId": 5,
                    "uri": "/EX-SELF"
                },
                {
                    "id": 14,
                    "menuLevel": 2,
                    "menuName": "考核评分",
                    "pMenuId": 5,
                    "uri": "/EX-RATE"
                },
                {
                    "id": 15,
                    "menuLevel": 2,
                    "menuName": "等级评定",
                    "pMenuId": 5,
                    "uri": "/EX-LEVEL"
                },
                {
                    "id": 16,
                    "menuLevel": 2,
                    "menuName": "考核反馈",
                    "pMenuId": 5,
                    "uri": "/EX-FEEDBACK"
                },
                {
                    "id": 17,
                    "menuLevel": 2,
                    "menuName": "角色管理",
                    "pMenuId": 5,
                    "uri": "/AD-ROLE"
                },
                {
                    "id": 18,
                    "menuLevel": 2,
                    "menuName": "用户管理",
                    "pMenuId": 5,
                    "uri": "/AD-USER"
                },
                {
                    "id": 19,
                    "menuLevel": 2,
                    "menuName": "部门管理",
                    "pMenuId": 5,
                    "uri": "/AD-DEP"
                },
                {
                    "id": 20,
                    "menuLevel": 2,
                    "menuName": "综合评级",
                    "pMenuId": 5,
                    "uri": "/AD-RATE"
                }
            ]
        }
    ]
}
````
## 获取用户列表
### url:
/user/list
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |必填｜
|  ----  | ----  | ----  |
| mobile  |手机号|否｜
| realName  |姓名|否｜
| teamName  |团队名，模糊查询|否｜
### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<User\>|


## 修改用户信息
### url:
/user/edit
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
| id| 用户id，必须填|
| mobile| 手机号|
| roleId| 角色编号|
|realName|姓名|
|departId|部门Id|
|teamId|团队id|
|email|邮箱地址|
### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||


## 删除用户
### url:
/user/delete
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
| mobile| 手机号|

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||


## 根据id查询用户信息（部门领导，团队领导查询考核页面详情的时候应该会用到）
### url:
/user/queryUserById
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
| userId| 用户Id|

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |User|


## 员工查看自己绩效考核的页面（绩效自评）
### url:
/performanceScore/view
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<PerformanceDetailScoreVo\>|

PerformanceDetailScoreVo

|  参数  |  描述 |
|  ----  | ----  |
| planId  | 绩效计划id|
| year  | 年份 |
| season  |季度（A，B，C，D）|
| kpi  |kpi|
| weight  |权重|
| indicatorDescription  |指标说明|
| yearGoal  |年度目标|
| goalA  |一季度目标|
| goalB  |二季度目标|
| goalC  |三季度目标|
| goalD  |四季度目标|
| scoreId  |绩效考核Id|
| sortIndex  |排序下标|
| selfScore  |绩效自评分|
| teamLeaderScore  |团队总监评分|
| departLeaderScore  |部门领导评分|


## 员工绩效自评
### url:
/performanceScore/selfScore
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  scoreVo  | List\<PerformanceDetailScoreVo\>  |

#### PerformanceDetailScoreVo

  参数  |  描述 |
|  ----  | ----  |
| planId  | 绩效计划id|
| year  | 年份 |
| season  |季度（A，B，C，D）|
| kpi  |kpi|
| weight  |权重|
| indicatorDescription  |指标说明|
| yearGoal  |年度目标|
| goalA  |一季度目标|
| goalB  |二季度目标|
| goalC  |三季度目标|
| goalD  |四季度目标|
| scoreId  |绩效考核Id|
| sortIndex  |排序下标|
| selfScore  |绩效自评分|
| teamLeaderScore  |团队总监评分|
| departLeaderScore  |部门领导评分|

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  ||

## 查看下属绩效考核列表
### url:
/performanceScore/queryStaffScoreList
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{"members":List\<PerformanceScore\>,"leaders":List\<PerformanceScore\>}|


```
团队总监查看只能有members字段
部门总监和副总能查到members 和leaders字段

```


#### PerformanceScore

|  参数  |  描述 |
|  ----  | ----  |
| id  | 绩效评定Id(后面打分的接口带回去)|
| userId  | 员工Id |
| realName  | 员工姓名 |
| roleId  | 员工角色Id |
| position  | 员工岗位 |
| teamId  | 员工团队id |
| teamName  | 员工团队名称 |
| departName  | 员工部门名称 |
| departId  | 员工部门id|
| year  | 绩效年份|
| season  | 绩效季度|
| selfScore  | 员工绩效自评分|
| teamLeaderScore  | 团队总监绩效打分|
| departLeaderScore  | 部门总监绩效打分|
| vicePresidentScore  | 副总绩效打分|
| performanceRank  | 员工绩效评级|
| feedback  | 员工绩效反馈|
| teamLeaderFeedback  | 团队总监对员工绩效反馈|



## 查看员工当前季度绩效的打分详情
### url:
/performanceScore/queryStaffScoreDetail
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
|  userId  | 下属用户id（[查看下属绩效考核列表]接口中返回）  |
|  scoreId  | 绩效考核Id （[查看下属绩效考核列表]接口中返回） |

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |List\<PerformanceDetailScoreVo\> |

## 团队总监，部门总监给下属绩效评分
### url:
/performanceScore/scoreToStaff
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
|  staffId  | 下属用户id（[查看员工当前季度绩效的打分详情]接口中返回）  |
|  scoreId  | 绩效考核Id （[查看员工当前季度绩效的打分详情]接口中返回） |
|  planId  | 下属绩效计划Id（[查看员工当前季度绩效的打分详情]接口中返回）  |
|  scoreVo  | List\<PerformanceScoreDetail\>  |

#### PerformanceScoreDetail

|  参数  |  描述 |
|  ----  | ----  |
|  selfScore  | 绩效自评分|
|  teamLeaderScore  | 团队总监打分|
|  departLeaderScore  | 部门总监打分|
|  sortIndex  | 下标|


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  | |


## 部门副总给员工总打分
### url:
/performanceScore/sumScoreToStaff
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |
### body
|  参数  |  描述 |
|  ----  | ----  |
|  score  |  List\<PerformanceScore\> performanceScores  |

PerformanceScore

|  参数  |  描述 |
|  ----  | ----  |
|  id  | 下属用户id（[queryStaffScoreList]接口中返回）  |
|  vicePresidentScore  | 部门副总打总分 |


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  | |


## 综合管理部获取部门评级列表
### url:
/performanceScore/departmentScoreList
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
|  data  | List\<PDepartmentScore\>  |

#### PDepartmentScore

|  参数  |  描述 |
|  ----  | ----  |
| year  | 年份|
| season  | 第几季度 |
|  departId  | departId  |
|  departmentName  | 部门名称 |
|  performanceRank  | 部门评级 |




## 综合管理部给部门打分
### url:
/performanceScore/scoreToDepartment
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
|  departId  |  部门Id |
|  departmentName  |  部门name |
|  performanceRank  |  部门评级（A，B，C，D） |


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  | |



## 综合管理部给部门打分
### url:
/performanceScore/getUserRankList
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |


### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{"members": List\<PerformanceScore\> ,"leaders": List\<PerformanceScore\> }|

团队总监只能看到members字段，部门总监和副总可以看到leaders字段


## 部门总监给员工评级
### url:
/performanceScore/rankToStaff
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
|  scores  |  List\<PerformanceScore\>  |

PerformanceScore

|  参数  |  描述 |
|  ----  | ----  |
|  id  | 下属用户id（[getUserRankList]接口中返回）  |
|  performanceRank  | 评级 A,B,C,D |

### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  | |


## 员工查看自己的绩效评分
### url:
/performanceScore/viewScoreRank
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |



### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |PerformanceScore |

#### PerformanceScore

|  参数  |  描述 |
|  ----  | ----  |
| id  | 绩效评定Id(后面打分的接口带回去)|
| userId  | 员工Id |
| realName  | 员工姓名 |
| roleId  | 员工角色Id |
| position  | 员工岗位 |
| teamId  | 员工团队id |
| teamName  | 员工团队名称 |
| departName  | 员工部门名称 |
| departId  | 员工部门id|
| year  | 绩效年份|
| season  | 绩效季度|
| selfScore  | 员工绩效自评分|
| teamLeaderScore  | 团队总监绩效打分|
| departLeaderScore  | 部门总监绩效打分|
| vicePresidentScore  | 副总绩效打分|
| performanceRank  | 员工绩效评级|
| feedback  | 员工绩效反馈|
| teamLeaderFeedback  | 团队总监对员工绩效反馈|



## 员工查看自己的绩效评分
### url:
/performanceScore/viewStaffScoreRank
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |



### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |“members”:List\<PerformanceScore\>,"leaders":List\<PerformanceScore\> |

团队总监只能看到members字段，部门总监和副总可以看到leaders字段


## 员工或者团队总监对绩效考核进行反馈
### url:
/performanceScore/feedback
### header:
|  参数  |  描述 |
|  ----  | ----  |
| Content-Length:  | application/json |
| userCode:  | 手机号|
| token | 登录接口返回的token |

### body
|  参数  |  描述 |
|  ----  | ----  |
|  scoreId  | viewStaffScoreRank 接口返回的id字段 |
|  feedback  | 反馈详情 |



### 返回

|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  | |
















