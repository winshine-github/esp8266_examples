by WangShuai
2018年11月24日17点50分
此工程测试系统的看门狗复位，工程在os_timer工程的基础上更改而来。
在no-OS SDk的开发情况下，不支持抢占任务或进程切换，
开发者需要自行保证程序的正确执行，用户代码不能长期占用CPU。
否则会导致看门狗复位，ESP8266重启，例如：在不喂狗的情况下使用while(1)；

如果在某些特殊情况下，用户线程必须执行较长时间（比如大于500ms），建议经常
调用system_soft_wdt_feed()API来喂狗，不建议禁止看门狗。
    while(1)    //这样会造成看门狗复位
    {
        system_soft_wdt_feed(); //这样就不会造成看门狗复位，但是会导致程序不再响应软定时器os_timer的回调
    }
猜测以上不会再响应os_timer()回调的原因是系统一直在喂狗。