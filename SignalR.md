# SignalR

The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server. In server code, you define methods that can be called by clients, and you call methods that run on the client. In client code, you define methods that can be called from the server, and you call methods that run on the server. SignalR takes care of all of the client-to-server plumbing for you.

As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.

### Groups
Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients. A group can have any number of clients, and a client can be a member of any number of groups. You don't have to explicitly create groups. In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.

### Hub object lifetime
You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline. SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.

Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next. Each time the server receives a method call from a client, a new instance of your Hub class processes the message. To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from Hub. If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.

If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.

##### How to specify query string parameters
If you want to send data to the server when the client connects, you can add query string parameters to the connection object. The following example shows how to set a query string parameter in client code.

```csharp
var querystringData = new Dictionary<string, string>();
querystringData.Add("contosochatversion", "1.0");
var connection = new HubConnection("http://contoso.com/", querystringData);

//The following example shows how to read a query string parameter in server code
public override Task OnConnected()
{
    var version = Context.QueryString["contosochatversion"];
    if (version != "1.0")
        Clients.Caller.notifyWrongVersion();
        
    return base.OnConnected();
}
```

###### References
https://docs.microsoft.com/en-us/aspnet/signalr/