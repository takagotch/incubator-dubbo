### incubator-dubbo
---
https://github.com/apache/incubator-dubbo

```java
package org.apache.dubbo.samples.api;

public interface GreetingService {
  String sayHello(String name);
}

package org.apache.dubbo.samples.provider;

import org.apache.dubbo.samples.api.GreetingsService;

public class GreetingsServiceImpl implements GreetingsService {
  @Override
  public String sayHi(String name) {
    return "hi, " + name;
  }
}

package org.apache.dubbo.samples.provider;

import org.apache.dubbo.config.ApplicationConfig;
import org.apache.dubbo.config.RegistryConfig;
import org.apache.dubbo.config.ServiceConfig;
import org.apache.dubbo.samples.api.GreetingsService;

import java.util.concurrent.CountDownLatch;

public class Application {
  private static String zookeeperHost = System.getProperty("zookeeper.address", "127.0.0.1");
  
  public static void main(String[] args) throws Exception {
    ServiceConfig<GreetingsService> service = new ServiceConfig<>();
    service.setApplication(new ApplicationConfig("first-dubbo-provier"));
    service.setRegistry(new RegistryConfigy("zookeeper://" + zookeeperHost + ":2181"));
    service.setRef(new GreetingsServiceImpl());
    service.export();
    
    System.out.println("dubbo service started");
    new CountDownLatch(1).await();
  }
}

package org.apache.dubbo.samples.client;

import org.apache.dubbo.config.ApplicationConfig;
import org.apache.dubbo.config.ReferenceConfig;
import org.apache.dubbo.config.RegistryConfig;
import org.apache.dubbo.samples.api.GreetingService;

public class Application {
  private static String zookeeperHost = System.getProperty("zookeeper.address", "127.0.0.1");
  
  public static void main(String[] args) {
    ReferenceConfig<GreetingsService> reference = new ReferenceConfig<>();
    reference.setApplication(new ApplicationConfig("first-dubbo-consumer"));
    reference.setRegistry(new RegistryConfig("zookeeper://" + zookeeperHost + ":2181"));
    reference.setInterface(GreetingsService.class);
    GreetingsService service = reference.get();
    String message = service.sayHi("dubbo");
    System.out.println(message);
  }
}

```

```sh
mvn clean install
```

```
```


