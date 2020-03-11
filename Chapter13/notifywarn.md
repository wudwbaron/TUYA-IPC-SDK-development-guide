# 自定义事件文字告警开发  
应用场景：  
当某个事件触发后，只希望给app推送一条文字告警信息，不需要推送图片，且包含的事件是完全自定义的，SDK中目前不存在，报警的文案可以自己设定  
开发流程：  
1、在iot平台新增一个dp点或者直接采取pid中的默认的dp点，本文以低电量告警为例  
2、在iot平台配置告警  
3、当事件触发后推送告警，以上报dp点方式推送告警  
例如设备持续上报电量值，当上报的值符合配置的告警条件，app就会收到低电量告警的推送  
4、如果没收到推送，首先在日志平台中查找设备上报的电量值是多少    