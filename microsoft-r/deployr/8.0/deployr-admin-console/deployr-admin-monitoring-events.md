# Monitoring Events

When you are logged in as admin, you can monitor the runtime grid and security events on the management event stream from the **Home** page.  The management event stream allows you to see runtime grid activity events, grid warning events, and security access events. By default this stream is accessible to admin only; however, access can be changed in the **Server Policies** tab.

![](media/deployr-admin-monitoring-events/03000023_612x363.png)  
*Home tab after login*

- Grid activity events that include the starting and stopping of grid operations, such as stateless, temporary, persistent projects and/or jobs
- Grid warning events that occur when the grid runs up against the limits that were defined in the **Concurrent Operation Policies** in the [Server Policies](https://deployr.revolutionanalytics.com/documents/help/admin-console/Content/Topics/policies-properties.htm) tab
- Grid heartbeat events that are pushed regularly and provide a detailed summary of slot activity across all nodes on the grid
- Security events that are pushed when a user attempts to log in (`securityLoginEvent event`) or log out (`securityLogoutEvent event`)