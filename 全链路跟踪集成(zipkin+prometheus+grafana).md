spring dubbo zipkin集成

1.启动zipkin服务

docker run -d -p 9411:9411 openzipkin/zipkin

2.spring boot及dubbo程序埋点，参数https://help.aliyun.com/document_detail/95862.html?aly_as=HjFzlwq#title-clh-ykn-eud

3.为了跟日志对应，在log4j配置日志里面输出traceId,spanId,配置里面加上[%X{traceId}/%X{spanId}]即可

4.如果想在zipkin后台根据用户ID或其它参数查询，则需要在程序中自定义tag如下

```
    // 获取tracing对象，创建 rootspan
		tracing.tracer().startScopedSpan("parentSpan");
		Span span = tracing.tracer().currentSpan();
		span.tag(ud.getUserId(), "userId");
    //业务代码
    span.finish();
    
```

5.即可在zipkin后台查询，http://host:9411/zipkin

6.由于zipkin无监控模块，可引入prometheus+grafana进行监控告警，操作如下

7.使用zipkin官方提供的prometheus配置文件https://github.com/openzipkin/docker-zipkin/blob/master/prometheus/prometheus.yml ，启动prometheus服务，定时从zipkin获取据数据

docker run --privileged=true -d --name prometheus -p 9090:9090 -v /apps/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

8.启动grafana服务

docker run -d --name grafana -p 3000:3000 grafana/grafana

9.登录grafana（默认用户密码admin/admin）,根据https://grafana.com/grafana/dashboards/1598/revisions 提供的模板添加Dashboard，即可完成监控，目前看监控参数不完整。
