# JPush API Golang

[![Go Report Card](https://goreportcard.com/badge/github.com/Krocsky/chaos-jpush-api)](https://goreportcard.com/report/github.com/Krocsky/chaos-jpush-api)
[![Go](https://github.com/Krocsky/chaos-jpush-api/actions/workflows/go.yml/badge.svg?branch=master)](https://github.com/Krocsky/chaos-jpush-api/actions/workflows/go.yml)

## 概述

这是 JPush REST API 的 Golang 版本封装开发包，不是由极光推送官方提供的，一般支持最新的 API 功能。

对应的 REST API 文档：<https://docs.jiguang.cn/jpush/server/push/server_overview/>

## 兼容版本

- Golang 1.15

## 环境配置

```bash
go get github.com/Krocsky/chaos-jpush-api
```

## 代码样例

> 代码样例在 jpush-api-golang 中的 example 文件夹中，[点击查看所有 example ](https://github.com/Krocsky/chaos-jpush-api/tree/master/example) 。

> 这个样例演示了消息推送。

```golang
j := jpush.NewJPush(Appkey, masterSecret)
aud := &jpush.PushAudience{}
aud.SetAll(true)
req := &jpush.PushRequest{
    Platform: &jpush.Platform{Platforms: []string{"android", "ios"}},
    Audience: aud,
    Notification: &jpush.PushNotification{
        Alert: "test alert",
        Android: &jpush.NotificationAndroid{
            Alert:     "alert",
            Title:     "title",
            BuilderID: 0,
            Priority:  1,
            AlertType: 7,
        },
    },
    Options: &jpush.PushOptions{
        TimeToLive: 0,
    },
}
ret, err := j.Push(req)
if err != nil {
    fmt.Println(err.Error())
    return
}
```

## HTTP 状态码

参考文档：<http://docs.jiguang.cn/jpush/server/push/http_status_code/>

Push v3 API 状态码 参考文档：<http://docs.jiguang.cn/jpush/server/push/rest_api_v3_push/>

Report API 状态码 参考文档：<http://docs.jiguang.cn/jpush/server/push/rest_api_v3_report/>

Device API 状态码 参考文档：<http://docs.jiguang.cn/jpush/server/push/rest_api_v3_device/>

Push Schedule API 状态码 参考文档：<http://docs.jiguang.cn/jpush/server/push/rest_api_push_schedule/>

[Release 页面](https://github.com/Krocsky/chaos-jpush-api/releases) 有详细的版本发布记录与下载。

## 根据之前作者的代码改写

新增字段 ThirdPartyChannel、ChannelID，参考文档：<https://docs.jiguang.cn/jpush/server/push/rest_api_v3_push#android>

```golang
// NotificationAndroid define android notification
type NotificationAndroid struct {
	Alert      string                 `json:"alert"`
	Title      string                 `json:"title,omitempty"`
	BuilderID  int                    `json:"builder_id,int,omitempty"`
	Priority   int                    `json:"priority,omitempty"`
	Category   string                 `json:"category,omitempty"`
	Style      int                    `json:"style,int,omitempty"`
	AlertType  int                    `json:"alert_type,int,omitempty"`
	BigText    string                 `json:"big_text,omitempty"`
	Inbox      map[string]interface{} `json:"inbox,omitempty"`
	BigPicPath string                 `json:"big_pic_path,omitempty"`
	Extras     map[string]interface{} `json:"extras,omitempty"`
	LargeIcon  string                 `json:"large_icon,omitempty"`
	Intent     map[string]interface{} `json:"intent,omitempty"`
	ChannelID  string                 `json:"channel_id,omitempty"`
}

// PushOptions define options
type PushOptions struct {
	SendNo            int                    `json:"sendno,int,omitempty"`
	TimeToLive        int                    `json:"time_to_live,int,omitempty"`
	OverrideMsgID     int64                  `json:"override_msg_id,int64,omitempty"`
	ApnsProduction    bool                   `json:"apns_production"`
	ApnsCollapseID    string                 `json:"apns_collapse_id,omitempty"`
	BigPushDuration   int                    `json:"big_push_duration,int,omitempty"`
	ThirdPartyChannel map[string]interface{} `json:"third_party_channel,omitempty"`
}
```
