# Active Directory Lab

## Overview

This lab focuses on implementing on-premises Active Directory within Azure virtual machines (on-premises Active Directory deployed in the cloud). The first portion of the lab involves creating and provisioning resources. The second portion of the lab involves configuring a domain controller and client. The third and final portion of the lab involves cleanup.  

### Create and Provision Resources 
* Create domain controller (Windows 2022 Server virtual machine) in Azure
* Create client (Windows 10 Professional virtual machine) in Azure
* Examine virtual network topology 

### Configure Domain Controller and Client
* Ensure connectivity between client and domain controller 
* Install Active Directory Domain Services on domain controller 
* Create administrator and normal user accounts in Active Directory 
* Join client to domain 
* Enable remote desktop connection for non-administrative users on client 
* Create additional user accounts that can log in to client 

### Lab Cleanup
* Close connections
* Delete resource groups

## Environments and Technologies Used
* Microsoft Azure
* Remote Desktop Connection
* Active Directory Domain Services
* PowerShell

## Operating Systems Used
* Windows Server 2022
* Windows 10 Professional 

## Create and Provision Resources 

### Create Domain Controller (Windows Server 2022 Virtual Machine) in Azure

> ![image](https://user-images.githubusercontent.com/15792522/200431383-0b53e5c9-9aab-4505-b228-a47f81955cd6.png)

> ![image](https://user-images.githubusercontent.com/15792522/200431695-566d287f-888d-46d5-af6b-e23c4a8a64f0.png)

Set the domain controller's NIC's private IP address to static. 

> ![image](https://user-images.githubusercontent.com/15792522/200433182-8b3ba4e8-6a2b-442b-8ea6-4d0d4db53674.png)

### Create Client (Windows 10 Professional Virtual Machine) in Azure

> ![image](https://user-images.githubusercontent.com/15792522/200432208-96c205d0-977e-47d9-8f14-70a069b0703f.png)

> ![image](https://user-images.githubusercontent.com/15792522/200432274-7d7bce4c-6525-4c98-b029-cce79d17f441.png)

> ![image](https://user-images.githubusercontent.com/15792522/200432438-8f92f160-d623-4f20-8fa5-cef4a1903149.png)

> ![image](https://user-images.githubusercontent.com/15792522/200432706-5a1a08f3-cb49-4ba6-aea1-b42a7c99eeb3.png)

### Examine Virtual Network Topology 

> ![image](https://user-images.githubusercontent.com/15792522/200433785-50ae9fab-3667-4a42-9ca1-fb9b934e1aa9.png)

## Configure Domain Controller and Client

### Ensure Connectivity Between Client and Domain Controller

Log in to the domain controller (Windows Server 2022 virtual machine) and enable ICMPv4 on the local **Windows Firewall**.

> ![image](https://user-images.githubusercontent.com/15792522/200435476-17162b61-297f-4cca-8c11-e00b23e6209b.png)

> ![image](https://user-images.githubusercontent.com/15792522/200436206-ae5574ca-a6bf-4539-9e3b-184fb9191966.png)

> ![image](https://user-images.githubusercontent.com/15792522/200436415-257d3fd2-5c29-47ce-8bac-c633e1adf8e0.png)

Log in to the client (Windows 10 virtual machine) and ensure that it can ping the domain controller.

> ![image](https://user-images.githubusercontent.com/15792522/200434331-5e154e58-393d-48eb-bb5a-21a9d1c5edf0.png)

> ![image](https://user-images.githubusercontent.com/15792522/200436531-b31ca2d3-71e7-4ad8-994c-4d72de732028.png)

### Install Active Directory Domain Services on Domain Controller 

Open **Server Manager** and install **Active Directory Domain Services**.

> ![image](https://user-images.githubusercontent.com/15792522/200437184-43c41a26-0690-45c7-9ddf-960930e73d20.png)

> ![image](https://user-images.githubusercontent.com/15792522/200437323-a8eafb03-a5a8-4556-b4e7-ccfdebb7c9c2.png)

After **Active Directory Domain Services** finishes installing, navigate to **Post-deployment Configuration** and promote the server to a domain controller.

> ![image](https://user-images.githubusercontent.com/15792522/200437653-4762f880-0bda-446d-b4da-71966bf1029c.png)

In the **Active Directory Domain Services Configuration Wizard**, add a new forest and enter a root domain name.

> ![image](https://user-images.githubusercontent.com/15792522/200437838-1736aed9-5eaa-4808-9712-678f6c7fa231.png)

Enter a Directory Services Restore Mode password and complete the remaining installation steps. 

> ![image](https://user-images.githubusercontent.com/15792522/200438142-5e9f92d4-d46b-401e-bac9-bf8167a2611e.png)

Allow the domain controller to restart, then log back in. This time, you'll need to specify the domain when logging in.  

> ![image](https://user-images.githubusercontent.com/15792522/200438907-62451a2c-5114-4359-9aaa-cbdc0a45b294.png)

### Create Administrator and Normal User Accounts in Active Directory 

In **Server Manager**, under **Tools**, select **Active Directory Users and Computers**. 

> ![image](https://user-images.githubusercontent.com/15792522/200439626-55694a95-5ee2-4b03-9b94-3a3901e20ce6.png)

Right-click on **mydomain** and select **New** > **Organizational Unit**. 

> ![image](https://user-images.githubusercontent.com/15792522/200440260-7941b19b-0f51-4317-8382-8bd0003299fd.png)

Create an organizational unit for employees.

> ![image](https://user-images.githubusercontent.com/15792522/200440442-c0f64a48-994a-477d-8195-69f273261823.png)

Create an organizational unit for admins. 

> ![image](https://user-images.githubusercontent.com/15792522/200440561-3a27d883-919e-407f-8ca6-4b5506ae64b2.png)

Create an admin user.

> ![image](https://user-images.githubusercontent.com/15792522/200440813-73142d3e-82f7-41ef-a95c-40544e77a930.png)

> ![image](https://user-images.githubusercontent.com/15792522/200441075-4628d59b-d736-4d28-a56a-aeb949df74cb.png)

For the purposes of this lab, we'll set a password that never expires. However, in practice, it's best to have the user create a new password upon logging in.  

> ![image](https://user-images.githubusercontent.com/15792522/200441246-00d644e0-e013-4ab9-8ad0-5df08de37066.png)

Once the admin account has been created, we'll need to change the user type to admin. Right-click on the user and click **Properties.** 

> ![image](https://user-images.githubusercontent.com/15792522/200441547-826971d9-a635-487e-ba52-bc4decbfefc8.png)

Add the user to the list of Domain Admins.

> ![image](https://user-images.githubusercontent.com/15792522/200441728-259fb3b4-24e0-4f98-9f29-0cfe73ce74d8.png)

Log out and log in as the new admin user. 

> ![image](https://user-images.githubusercontent.com/15792522/200442087-1765c365-93cf-405f-b410-eaa0b94274a5.png)

> ![image](https://user-images.githubusercontent.com/15792522/200442136-9056f6d1-1b68-4016-a87d-b25e065640b1.png)

> ![image](https://user-images.githubusercontent.com/15792522/200442309-bf9849a9-5e4a-47f0-bb73-29d3dda1efb7.png)

### Join Client to Domain

From the **Azure Portal**, set the client's DNS settings to the domain controller's private IP address. 

> ![image](https://user-images.githubusercontent.com/15792522/200443326-8cd7ff21-cba3-44b9-b219-e276f9381b06.png)

From the **Azure Portal**, restart the client. 

> ![image](https://user-images.githubusercontent.com/15792522/200443572-bfaa0ec3-9dac-4a78-a286-61dc84d5875a.png)

Log back into the client, then make the client a member of the domain.

> ![image](https://user-images.githubusercontent.com/15792522/200444059-9d33f0fc-9dc6-4ae6-8b15-65d894d53914.png)

Once prompted to log in, enter the credentials for the admin user.

> ![image](https://user-images.githubusercontent.com/15792522/200444199-47dd5da9-6d21-46c7-b414-c8b8d442c3bf.png)

> ![image](https://user-images.githubusercontent.com/15792522/200444259-698012d1-a518-4293-a097-112d7595202e.png)

Restart the client to complete the process of joining the client to the domain, then log in to the client as the admin user.

![image](https://user-images.githubusercontent.com/15792522/200444514-37f16426-cfcc-4355-adf0-c04731264ab5.png)

### Enable Remote Desktop Connection for Non-administrative Users on Client

Allow domain users to connect to client via **Remote Desktop Connection**. 

> ![image](https://user-images.githubusercontent.com/15792522/200463979-3291bdc2-de46-4b69-a893-0b729bb3cc1d.png)

View list of domain users in the domain controller. 

> ![image](https://user-images.githubusercontent.com/15792522/200464374-9ee19f8a-13ff-44a8-ac64-e48eba52e06c.png)

### Create Additional Users that Can Log In to Client

In the domain controller, open **PowerShell ISE** as an administrator, then create a new file with this script:

```PowerShell
 # ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 1000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}
```

> ![image](https://user-images.githubusercontent.com/15792522/200465718-d033a3ac-451e-4e77-92ef-1df6355fdaca.png)

This script creates 1,000 random accounts. To run this script, press the green play button.  

> ![image](https://user-images.githubusercontent.com/15792522/200465801-354c8f31-5cb8-4a57-8a1f-700c6b511fea.png)

Open **Active Directory Users** and Computers and click on the **\_EMPLOYEES** group to view the random users that were generated by the script.

> ![image](https://user-images.githubusercontent.com/15792522/200466383-c01611aa-c146-4860-8148-aea2950d5c86.png)

Log in to the client as one of the randomly generated users. 

> ![image](https://user-images.githubusercontent.com/15792522/200466737-7da269ae-03d1-4fdb-bad7-4afc221f56e8.png)

Perform an account unlock for one of the randomly-generated users. Right-click on their name, go to **Properties**, select **Account**, and then check the **Unlock account** box.  

> ![image](https://user-images.githubusercontent.com/15792522/200467288-f806f1ef-c934-4e53-989a-cfcec0c938d1.png)

Perform a password reset and account unlock for one of the randomly-generated users. Right-click on their name, select **Reset Password...**, and enter a new password. You can also require the user to change their password at next logon by checking the **User must change password at next logon** box and unlock the user's account by checking the **Unlock the user's account** box.  

> ![image](https://user-images.githubusercontent.com/15792522/200467536-afb2ff21-b98e-4b0f-805f-c3a60580e396.png)

> ![image](https://user-images.githubusercontent.com/15792522/200467703-14680d62-11d9-48cd-920a-d2296819e8f9.png)

> ![image](https://user-images.githubusercontent.com/15792522/200467858-25fe4b6b-d6f8-413c-b13b-44d55a18b1eb.png)

Disable a randomly-generated user's account. Right-click on their name and select **Disable Account**. 

> ![image](https://user-images.githubusercontent.com/15792522/200468018-cf425b85-7c67-44dc-9e89-1add96335c80.png)

> ![image](https://user-images.githubusercontent.com/15792522/200468072-492c9000-2070-483d-abf8-cdb2f6b025f9.png)

## Lab Cleanup

### Close Connections 

> ![image](https://user-images.githubusercontent.com/15792522/200468436-d637de6d-0c47-42ac-8f11-797acfa11ded.png)

### Delete Resource Groups

> ![image](https://user-images.githubusercontent.com/15792522/200468610-ddba33ad-2890-4303-959d-6915cc8fcf03.png)

> ![image](https://user-images.githubusercontent.com/15792522/200468705-f90868e4-f70e-40d8-a288-b375d97e2cf0.png)

> ![image](https://user-images.githubusercontent.com/15792522/200468826-6d626f89-e480-4c87-be72-18d3aa1380c5.png)

> ![image](https://user-images.githubusercontent.com/15792522/200468886-b76d557b-1f1f-420a-b9f9-448150c4c43b.png)
