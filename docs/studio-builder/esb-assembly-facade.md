In Integration Runtime (IR1), you could initialize almost any assembly class using `new` on the concrete class. This is not the case in Integration Runtime 2 (IR2). The single point of entry to instantiate Integration Runtime 2 Objects is the new class `EsbAssemblyFacade`.

Call the `getInstance` method to get an instance of the `EsbAssemblyFacade`. Then invoke the factory methods to instantiate the required object.

This table compares instantiation of the `MediationHelper` object in Integration Runtime 1 and Integration Runtime 2:

**Integration Runtime 1**
```
import com.capeclear.mediation.MediationHelper;  
import com.capeclear.mediation.impl.DefaultMediationHelper;  

public class Test() {  
  public void testMethod() { 
     MediationHelper helper = new DefaultMediationHelper();  
     }
}
```

**Integration Runtime 2**
```
import com.workday.assembly.runtime.api.EsbAssemblyFacade;
import com.workday.assembly.runtime.api.mediation.MediationHelper;
 
public class Test() {
  public void testMethod() {
     EsbAssemblyFacade instance = EsbAssemblyFacade.getInstance();
     MediationHelper mediationHelper = instance.getMediationHelper();
     }
}
```

### EsbAssemblyFacade Method Reference

This table describes the methods available to `EsbAssemblyFacade`.

| Method | Description |
| :--- | :--- |
| createBlobitoryClient<br>(com.workday.assembly.runtime.api.mediation.<br>MediationContext context) | Returns the BlobitoryClient, which enables you to store and retrieve documents using Java.<br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>blobitory.BlobitoryClient |
| createDataHandlerSource<br>(javax.activation.DataHandler dataHandler) | Creates a new data handler source from a DataHandler.<br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>mediation.DataHandlerSource |
| createDataHandlerSource<br>(javax.activation.DataSource dataSource) | Creates a new data handler source from a DataSource.<br>**Return Type:**<br>com.workday.assembly.runtime.api.mediation.<br>DataHandlerSource |
| createManagedData<br>(java.lang.String mimeType) | Creates a new managed data.<br>**Return Type:**<br>com.workday.assembly.runtime.api.ManagedData |
| createManagedData<br>(java.lang.String mimeType, java.lang.<br>String name) | Creates a new managed data with a name.<br>**Return Type:**<br>com.workday.assembly.runtime.api.ManagedData |
| createXmlStreamMatcher<br>() | Creates and return a XMLStreamMatcher instance.<br>**Return Type:**<br>com.workday.assembly.runtime.api.util.<br>XMLStreamMatcher |
| createDaemonThreadFactory<br>(java.lang.String name) | Creates and returns a new thread factory.<br>**Return Type:**<br>java.util.concurrent.ThreadFactory |
| createResourcePool<br>(java.lang.String name, ResourcePool.ResourceFactory factory) | Creates and returns a new resource pool with a name.<br>**Return Type:**<br>ResourcePool |
| createResourcePool<br>(java.lang.String name, ResourcePool.ResourceFactory factory,<br>int cacheSize) | Creates and returns a new resource pool with a name and cache size.<br>**Return Type:**<br>ResourcePool |
| createClientContext<br>() | Creates and returns a new client context.<br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>client.ClientContext |
| getAssemblyUtils<br>() | Returns the AssemblyUtils instance.<br><br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>AssemblyUtils |
| getCallContextFactory<br>() | Returns the CallContextFactory instance.<br><br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>application.CallContextFactory |
| getInstance<br>() | Returns the system-wide toggle service instance.<br><br>**Return Type:**<br>EsbAssemblyFacade |
| getIntegrationModelFactory<br>() | Returns the Integration Model Factory.<br><br>**Return Type:**<br>com.workday.assembly.<br>runtime.api.integration.<br>IntegrationModelFactory |
| getManagedDataFromSource<br>(javax.xml.transform.Source source) | Returns ManagedData from Source.<br><br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>ManagedData |
| getMediationHelper<br>() | Returns the MediationHelper instance.<br><br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>mediation.MediationHelper |
| getMediationTube<br>() | Returns the MediationTube instance.<br><br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>mediation.MediationTube |
| getMimeTypeHelper<br>() | Returns a MimeTypeHelper with utility methods for working with mimetypes, file extensions, and charsets.<br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>transport.MimeTypeHelper |
| getStatsHelper<br>() | Creates and returns a StatsHelper instance.<br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>stats.StatsHelper |
| getXmlStreamUtils<br>() | Returns the XmlStreamUtils instance.<br>**Return Type:**<br>com.workday.assembly.runtime.api.<br>util.XmlStreamUtils |