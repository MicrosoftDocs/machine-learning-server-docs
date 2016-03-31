# Enabling DeployR on Azure

## Introduction

You can set up DeployR on **Microsoft Azure**. For each Azure instance, be sure to:

1.  Set the [server Web context](#set-context-azure) to an external, public IP address.

2.  Set the appropriate security permissions on the internal and external ports used by DeployR in the firewall:

    -   Open the DeployR external ports by adding [Azure endpoints](#endpoints).

    -   Update the [firewall](#firewall).


>[!IMPORTANT]
>[Change all default DeployR user passwords](https://deployr.revolutionanalytics.com/documents/admin/install/#change-pass), including the `admin` account, after installing DeployR.

## Setting the Server Web Context

The DeployR server Web context must be updated to the Public IP address of the virtual machine.

 
**To update the DeployR server Web context:**

### On Windows:

1.  Log into the Azure portal and take note of the **Public IP address**.

    ![Public IP Address in Azure Portal](./media/deployr-admin-configure-for-azure/azure-public-ip.png)

2.  If DeployR was installed on a virtual machine, remote desktop into that VM.

3.  Open a Command Window with **“Run as Administrator”**.

4.  Go to the DeployR tools directory under the DeployR installation directory. By default, this directory is `C:\Program Files\Microsoft\DeployR\8.0\deployr\`

        cd C:\Program Files\Microsoft\DeployR\8.0\deployr\tools

5.  Make sure that the MongoDB database is running. You can do so in the **Services** dialog by verifying that the database service has a status of `Started` or `Running`. The database must be running before you can proceed to the next step.

6.  Set the appropriate public IP using the `setWebContext.bat` script where `<ip_address>` is the public IP address of the machine. [Learn more about the script arguments](https://deployr.revolutionanalytics.com/documents/admin/troubleshoot/#set-context).

        setWebContext -ip <ip_address>

7.  Confirm the IP address you entered.

8.  Disable any automatic IP detection that might overwrite the IP you just assigned.

        setWebContext -disableauto

9.  For this change to take effect [restart the Tomcat service](https://deployr.revolutionanalytics.com/documents/admin/common/#server). Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.

### On Linux:

1.  Log into the Azure portal and take note of the **Public IP address**.

    ![Public IP Address in Azure Portal](./media/deployr-admin-configure-for-azure/azure-public-ip.png)

2.  If DeployR was installed on a virtual machine, SSH into that machine.

3.  Go to the DeployR tools directory under the DeployR installation directory. By default, this directory for the `deployr-user` is `/home/deployr-user/deployr/8.0.0/deployr/`

        cd /home/deployr-user/deployr/8.0.0/deployr/tools

4.  Run the following command to check that the database is running. If the `ps -ef | grep mongo` command returns only a single line ressembling this `500      61997 61965  0 18:56 pts/0    00:00:00 grep mongo`, then the database is not running. The database must be running before proceeding to the next step.

        ps -ef | grep mongo

5.  Set the IP using the `setWebContext.sh` script where `<ip_address>` is the public IP address of the machine. [Learn more about the script arguments](https://deployr.revolutionanalytics.com/documents/admin/troubleshoot/#set-context).

        ./setWebContext.sh -ip <ip_address>

6.  Confirm the IP address you entered.

7.  Disable any automatic IP detection that might overwrite the IP you just assigned.

        ./setWebContext.sh -disableauto

8.  For this change to take effect [restart the DeployR services](https://deployr.revolutionanalytics.com/documents/admin/common/#server). Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.

---------

>[!IMPORTANT]
>We highly recommended that you also enable HTTPS support for DeployR to secure the communications to the server. See these instructions for [enabling HTTPS](https://deployr.revolutionanalytics.com/documents/admin/security/#httpson).

## Configuring Azure Endpoints

When provisioning your DeployR server on Azure, you must open Azure endpoints for several [DeployR ports](https://deployr.revolutionanalytics.com/documents/admin/install/#update-firewall). For DeployR 8.0.0, these ports are:

-   DeployR HTTP port: 8000
-   DeployR HTTPS port: 8001
-   DeployR event console port: 8006

>[!NOTE]
>If custom ports were defined during installation, enable those instead.

 
**To configure Azure endpoints for DeployR:**

1.  Go to the main Microsoft Azure portal page.

2.  Click the **Resource Group** name.

    ![Rules](./media/deployr-admin-configure-for-azure/azure-resource-group.png)

3.  In the table in the **Resource Group** page, click the **Network Security Group**.

4.  In the **Network Security Group** page, click **All Settings** option.

5.  Choose **Inbound security rules**.

6.  Click the **Add** button to create an inbound security rule for each DeployR port as follows:

    1.  In the **Add inbound security rule** page, enter a unique name the rule.

    2.  Set the protocol to `Any`.

    3.  Set the **Source Port Range** to the `*` character.

        ![Rules](./media/deployr-admin-configure-for-azure/azure-source-port-range.png)

    4.  Enter the port number to the **Destination port range**.

        -   For the DeployR HTTP port, enter `8000`.

        -   For the DeployR HTTPS port, enter `8001`.

        -   For the DeployR event console port, enter `8006`.

    5.  Click **OK** to save your changes.

    6.  Repeat step 6 to add inbound rules for the other DeployR ports.

![Rules](./media/deployr-admin-configure-for-azure/azure-inbound-rules.png)

## Updating the Firewall

Updating your firewall is the last step. [Learn more about the DeployR ports](https://deployr.revolutionanalytics.com/documents/admin/install/#update-firewall)

 
**To update your firewall:** []()

### For Windows:

In Windows Firewall, you must open the same DeployR ports as you configured as Azure endpoints.

1.  From the **Control Panel**, open the Window Firewall.

2.  Click **Advanced Settings**. The **Windows Firewall with Advanced Security** dialog appears.

3.  Choose **Inbound Rules**. The list of inbound rules appears.

4.  For DeployR HTTP port `8000`:

    1.  Open the rule called `RevoDeployR-Enterprise 8.0.0 Tomcat - 8000`.

    2.  Go to the **Advanced** tab.

    3.  Select the **Public** checkbox to enable public for this rule.

    4.  Click **OK**.

5.  For DeployR HTTPS port `8001`:

    1.  Open the rule called `RevoDeployR-Enterprise 8.0.0 Tomcat SSL - 8001`.

    2.  Go to the **Advanced** tab.

    3.  Select the **Public** checkbox to enable public for this rule.

    4.  Click **OK**.

6.  For DeployR event console port `8006`:

    1.  Open the rule called `RevoDeployR-Enterprise 8.0.0 Console Socket Server - 8006`.

    2.  Go to the **Advanced** tab.

    3.  Select the **Public** checkbox to enable public for this rule.

    4.  Click **OK**.

### For Linux:

On Linux, you must disable `iptables` firewall or equivalent.


