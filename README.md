# TACACS Authorization Accounting
## Authorization

Once AAA is enabled on a Cisco IOS device and the aaa authentication command is configured, you can optionally configure the dependent AAA functions aaa authorization and aaa accounting.

Configure AAA authorization using a named method list with a server group and fallback to local authentication.
```
Router(config)# aaa authorization exec MYTACAUTH group TACSRVGROUP local if-authenticated
Router(config)# aaa authorization commands 15 MYTACAUTH group TACSRVGROUP local
Router(config)# aaa authorization config-commands
```
Configure authorization using a named method applied to the vty lines.
```
Router(config)# line vty 0 4 
Router(config-line)# authorization exec MYTACAUTH
Router(config-line)# authorization commands 15 MYTACAUTH
```

### Authorization format
```
Router(config)# aaa authorization {commands | config-commands |  configuration | exec | network | reverse-access} {default | list-name} method1 [method2 ...]
```
Here you can specify the function or service needing authorization with one of the following keywords:

1. commands: The server must return permission to use any router command at any privilege level.
2. config-commands: The server must return permission to use any router configuration command.
3. exec: The server must return permission for the user to run a router EXEC session. The server also can return the privilege level for the user so that the user immediately can be put into privileged EXEC (enable) mode without having to type in the enable command.
4. if-authenticated—Requests are granted if the user already is authenticated.

You can identify the method with a descriptive name (list-name) if you are configuring more than one list. Otherwise, a single unnamed list is called the default list. Each authorization method then is listed in the order it will be tried.

### Apply the Authorization format
Next, you can apply an authorization method list to a specific line on the router. Users accessing the router through that line will be subject to authorization. Use the following line configuration command:
```
Router(config-line)# authorization {commands level | exec | reverse-access} {default | list-name}
```
If you do not use this command, the default group is used for all lines.

## Accounting

Configure AAA accounting using a named method list with a server group.
```
Router(config)# aaa accounting exec MYTACACC start-stop group TACSRVGROUP
Router(config)# aaa accounting commands 15 MYTACACC start-stop group TACSRVGROUP
```
Configure accounting using a named method applied to the vty lines.
```
Router(config)# line vty 0 4
Router(config-line)# accounting exec MYTACACC
Router(config-line)# accounting commands 15 MYTACACC
```
Cisco devices also support the capability to use AAA for producing accounting information of user activity. This accounting information can be collected by RADIUS and TACACS+ servers. Again, the RADIUS and TACACS+ servers must already be configured and grouped as part of the authentication configuration.

As usual, you must define a method list giving a sequence of accounting methods by using the following global configuration command:

### Accounting format
```
Router(config)# aaa accounting {system | exec | commands level} {default | list-name} {start-stop | stop-only | wait-start | none} method1 [method2...]
```
The function triggering the accounting can be one of the following keywords:

1. system—Major router events such as a reload are recorded.
2. exec—User authentication into an EXEC session is recorded, along with information about the user address and the time and duration of the session.
3. commands level—Information about any command running at a specific privilege level is recorded, along with the user who issued the command.

You can specify that certain types of accounting records be sent to the accounting server using the following keywords:

1. start-stop—Events are recorded when they start and stop.
2. stop-only—Events are recorded only when they stop.
3. none—No events are recorded.

### Apply the Accounting format
Next, you can apply an accounting method list to a specific line (console or vty) on the router. Users accessing the router through that line will have their activity recorded. Use the following line-configuration command to accomplish this:
```
Router(config-line)# accounting {commands level | connection |  exec} {default | list-name}
```
If you do not use this command, the default group, if any, will be used for all lines.
