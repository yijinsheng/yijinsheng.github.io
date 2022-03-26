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
/performance/platfr
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
| vcode  | 手机号 |
### 返回
|  参数  |  描述 |
|  ----  | ----  |
| code  | 返回码|
| message  | 返回消息 |
| data  |{token:登录认证token，userCode:用户编码} |
