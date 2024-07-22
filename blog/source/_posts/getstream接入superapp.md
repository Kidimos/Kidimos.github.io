---
title: getstream接入superapp
date: 2023-07-25 14:55:39
tags:
- 安卓开发
- 学习
- 编程
categories:
- 学习
- 编程
- 安卓开发

---

目的：将getstream中的feeds模块接入superapp中，实现新闻流自动推送（？

{% label 由于本项目使用Kotlin而不使用Java开发，所以我们使用两种不同的语言来作为例子 red %}

1. 首先，我们需要构建一个类似Twitter的可扩展新闻源，这个新闻源可以显示我们关注的人员的活动，这就是推送新闻的基础。

------

{% codeblock 创建新闻源 lang:java %}

// See https://github.com/getstream/stream-java for install instructions

// Initialize the client with your api key and secret.
Client client = Client.builder("xxx", "xxx").build();
// Get the feed object
FlatFeed ericFeed = client.flatFeed("user", "eric");
// Add the activity to the feed
Activity activity = Activity.builder()
        .actor("eric")
        .verb("tweet")
        .object("1")
        .extraField("tweet", "Hello world")
        .build();
ericFeed.addActivity(activity).join();

{% endcodeblock %}

------

2. 接下来，我们通过调用API来实现follow功能

{% codeblock follow lang:java %}

// Let Jessica's flat feed follow Eric's feed
FlatFeed jessicaFlatFeed = client.flatFeed("timeline", "jessica");
FlatFeed ericFlatFeed = client.flatFeed("user", "eric");
jessicaFlatFeed.follow(ericFlatFeed).join();

{% endcodeblock %}

{% codeblock follow lang:java %}

// Get the feed for Jule's aggregated feed and follow Eric's user feed

// Read the activities from Jessica's flat feed
List<Activity> response = jessicaFlatFeed.getActivities(new Pagination().limit(3)).get();

{% endcodeblock %}

------

3. 实现”聚合“功能（？

分为{% label flat blue%}、{% label aggregated red%}和{% label notification pink %}，有一说一不是很懂这三个代表什么

##### 让我们为 Jule 设置一个聚合提要，它将跟随 Eric 的更新。默认情况下，聚合信息源将按动词和日期聚合。您可以在仪表板中配置聚合规则。

{% codeblock follow lang:java %}

// Get the feed for Jule's aggregated feed and follow Eric's user feed
AggregatedFeed juleAggregatedFeed = client.aggregatedFeed("timeline_aggregated", "jule");
juleAggregatedFeed.follow(ericFlatFeed).join();

{% endcodeblock %}

成功！现在，当 Eric 观看两个视频时，它们就会出现在 Jessica 和 Jule 的信息源中。

{% codeblock follow lang:java %}

// Add an activity with the verb watch to Eric's feed
ericFlatFeed.addActivity(Activity.builder()
        .actor("eric")
        .verb("watch")
        .object("1")
        .extraField("youtube_id", "xxx")
        .extraField("video_name", "xxx")
        .build()).join();

{% endcodeblock %}

Success! Jule's feed aggregates the activities. In this case by the verb and date of the activity. You can read her feed as follows:

{% codeblock follow lang:java %}

// Read the last 3 activities from Jule's aggregated feed
List<? extends Group<Activity>> response = juleAggregatedFeed.getActivities(new Pagination().limit(3));

{% endcodeblock %}

您可以在仪表板中配置基本的聚合规则。下一步我们将介绍如何实时监听 feed 的变化。

------

4. 实时更新

{%codeblock realtime lang:java %}

// Listening to realtime updates is only available in JS
// You can pass the feed token as follows
// Generating tokens for client side usage
Token token = client.frontendToken("eric");
// Javascript client side feed initialization
// ericFeed = client.feed('user:eric', '{{ token }}');

{% endcodeblock %}

{%codeblock realtime lang:JavaScript%}

// Listen to feed changes in realtime
const displayModal = callback;
const subscription = ericFeed.subscribe(function(data){
  console.log("Eric's feed was updated!", data);
  displayModal(data);
});

// Add an activity when the websocket is ready
subscription.then(function() {
	ericFeed.addActivity({actor:'SU:eric', verb: 'tweet', object: 2, tweet: 'Cool stuff!'});
});

{% endcodeblock %}

------

### 需要注意的点

1. 在安卓端开发的时候，创建的Client为CloudClient，这要求我们提供每个登录用户的token。
2. token有两种获取的方式：
   1. 创建一个Client，从Client处获取令牌
   2. 通过网站来获取令牌
   3. 由于依赖冲突（主要是Base64）的问题，不知道为什么没有办法在本地获取令牌，所以我们使用从网站获取令牌的方法
3. 获取token后可以创建CloudClient，通过CloudClient可以获取Activities



### 需要做的事

1. 解决本地拿不到token的问题
   1. 目前的问题是依赖冲突无法解决，发邮件去问了getStream官方，希望能有回复（顺便问了一些Java教程出错了的问题）
2. 解决本地无法发布新的Activity的问题
   1. 已经解决了，关键在于Java教程的actor是错误的，正确格式应为"SU:"+userId.
3. 需要完成自定义字段
   1. 已经完成，使用Extra来装载自定义字段（自定义字段的格式为HashMap，官方文档给的是ImmutableMap，但是我又发生冲突了，发现另外注意到这是不可变字段，也许在自定义Views的时候HashMap会是更好的选择。
