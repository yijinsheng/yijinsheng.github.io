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
| performancePlanDetails| List<PerformancePlanDetail>|
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
| data  |List<PerformancePlanDetail>|

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
| state  | 流程状态，0-保存 ，1-团队总监审核中，2-部门总监审核中，3-副总审核中，4，完成状态，状态为0时候agreementTeam=2为团队总监驳回，agreementDepart 为部门总监驳回，agreementVice=2为副总驳回|
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
| data  |List<PerformancePlanDetail>|

## 负责任查看下属绩效计划列表
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
| data  |{"members":List<PerformancePlan>,"teamLeaders":List<PerformancePlan>}|

如果当前登录角色是团队总监，返回结果只有members，如果是部门总监则可以看到teamleaders和members，如果是副总，只会返回teamLeaders

## 负责人查看下属绩效计划详情
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
| performanceCts  | List<PerformanceCt>|
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
| data  |{"members":List<User>,"leaders":List<User>}|
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
| data  |List<PerformanceCt>|




