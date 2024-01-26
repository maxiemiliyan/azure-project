# IAM Implementation in Azure with Azure AD

## Project Overview

This project demonstrates the implementation of Identity and Access Management (IAM) in Microsoft Azure using Azure Active Directory (Azure AD). It includes the creation of users, assignment of different roles using Role-Based Access Control (RBAC), configuration of Conditional Access policies, and enabling Multi-Factor Authentication (MFA) for each user.

## Table of Contents

- [Technologies Used](#technologies-used)
- [Setup Instructions](#setup-instructions)
- [Create Users and Assign Roles](#create-users-and-assign-roles)
- [Implement Role-Based Access Control (RBAC)](#implement-role-based-access-control-rbac)
- [Define Conditional Access Policies](#define-conditional-access-policies)
- [Enable Multi-Factor Authentication (MFA)](#enable-multi-factor-authentication-mfa)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Technologies Used

- Microsoft Azure
- Azure Active Directory
- Role-Based Access Control (RBAC)
- Conditional Access
- Multi-Factor Authentication (MFA)

## Setup Instructions

### Create Users and Assign Roles

1. **User1 - Contributor Role:**
   - Assign the `Contributor` role to a specific resource group.

2. **User2 - Reader Role:**
   - Assign the `Reader` role to the entire subscription.

3. **User3 - Custom Role:**
   - Assign a custom role with specific permissions to a resource.

4. **User4 - User Access Administrator Role:**
   - Assign the `User Access Administrator` role.

### Implement Role-Based Access Control (RBAC)

Used Azure Portal to assign roles. 

1.User1 has the role of "Contributor"
 if a user has the "Contributor Role" in Azure RBAC, they have permissions to manage Azure resources within a resource group, but their access to applications and data is determined by Conditional Access policies, which are configured separately.Users with the Contributor Role can create, update, and delete Azure resources within the specified scope . This includes virtual machines, storage accounts, databases, etc.While Contributors have significant control over resources, they do not have permissions to manage access control (RBAC). This means they cannot assign roles to other users or modify access control settings for themselves or others.

2.User3 has the role of "Reader"
Assign the Reader role to users who require read-only access to view resources and configurations within a specific resource group or scope.Readers can see the details of resources but cannot make changes or deploy new resources.

3.User3 has the role of "Custom Role"
Use the Azure portal to define the custom role.Navigate to the Azure Portal, go to "Azure Active Directory," then "Roles and administrators."Click on "New custom role" and define the role with a name  "LimitedVMAdmin."

4.User4 has the role of "User Access Administrator"
The primary function of the "User Access Administrator" is to assign roles to other users, groups, or service principals. They can grant permissions such as Contributor, Reader, or custom roles to control access to specific resources.It provides significant access to control who has access to resources.

### Define Conditional Access Policies

1. **Conditional Access for User1 (Contributor):**
   ```json
   {
     "ConditionalAccessPolicy_User1": {
       "conditions": {
         "user": {
           "include": ["bccdbe9c-5628-41c8-b933-cf19fe8561d4"]
         }
       },
       "grant": {
         "actions": ["user_impersonation"],
         "resources": ["/subscriptions/baff284b-b98d-4a50-ab18-1d1ae16022b9/resourceGroups/demo"]
       }
     }
   }
2. Conditional Access for User2 (Reader):
   ```json
     {
      "ConditionalAccessPolicy_User2": {
        "conditions": {
          "user": {
            "include": ["0c9a00b6-38d0-4222-b6c7-903489564b19"]
          }
        },
        "grant": {
          "actions": ["read"],
          "resources": ["/subscriptions/baff284b-b98d-4a50-ab18-1d1ae16022b9/resourceGroups/demo"]
        }
      }
    }

3. Conditional Access for User3 (Custom Role):
   ```json
    {
      "ConditionalAccessPolicy_User3": {
        "conditions": {
          "user": {
            "include": ["e5a53cc7-25dd-421b-bfa4-07b440b20436"]
          }
        },
        "grant": {
          "actions": ["custom_permissions"],
          "resources": ["/subscriptions/baff284b-b98d-4a50-ab18-1d1ae16022b9/resourceGroups/demo"]
        }
      }
    }

4. Conditional Access for User4 (User Access Administrator):
   ```json
     {
    "ConditionalAccessPolicy_User4": {
      "conditions": {
        "user": {
          "include": ["d4b494fd-e9ea-4b66-9970-9637d57f0cef"]
        }
      },
      "grant": {
        "actions": ["user_access_admin"],
        "resources": ["/subscriptions/baff284b-b98d-4a50-ab18-1d1ae16022b9/resourceGroups/demo"]
      }
    }
   }

## Enable Multi-Factor Authentication (MFA)
