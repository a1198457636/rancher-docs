---
title: Adding Users to Projects
---

If you want to provide a user with access and permissions to _specific_ projects and resources within a cluster, assign the user a project membership.

You can add members to a project as it is created, or add them to an existing project.

:::tip

Want to provide a user with access to _all_ projects within a cluster? See [Adding Cluster Members](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/cluster-and-project-roles.md) instead.

:::

### Adding Members to a New Project

You can add members to a project as you create it (recommended if possible). For details on creating a new project, refer to the [cluster administration section.](../manage-clusters/projects-and-namespaces.md)

### Adding Members to an Existing Project

Following project creation, you can add users as project members so that they can access its resources.

1. In the upper left corner, click **☰ > Cluster Management**.
1. On the **Clusters** page, go to the cluster where you want to add members to a project and click **Explore**.
1. Click **Cluster > Projects/Namespaces**.
1. Go to the project where you want to add members and click **⋮ > Edit Config**.
1. In the **Members** tab, click **Add**.
1. Search for the user or group that you want to add to the project.

    If external authentication is configured:

    -  Rancher returns users from your external authentication source as you type.

    - A drop-down allows you to add groups instead of individual users. The dropdown only lists groups that you, the logged in user, are included in.

    :::note

    If you are logged in as a local user, external users do not display in your search results.

    :::

1. Assign the user or group **Project** roles.

    [What are Project Roles?](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/cluster-and-project-roles.md)

    :::note Notes:

    - Users assigned the `Owner` or `Member` role for a project automatically inherit the `namespace creation` role. However, this role is a [Kubernetes ClusterRole](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole), meaning its scope extends to all projects in the cluster. Therefore, users explicitly assigned the `Owner` or `Member` role for a project can create or delete namespaces in other projects they're assigned to, even with only the `Read Only` role assigned.

    - By default, the Rancher role of `project-member` inherits from the `Kubernetes-edit` role, and the `project-owner` role inherits from the `Kubernetes-admin` role. As such, both `project-member` and `project-owner` roles will allow for namespace management, including the ability to create and delete namespaces.

    - For `Custom` roles, you can modify the list of individual roles available for assignment.

        - To add roles to the list, [Add a Custom Role](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/custom-roles.md).
        - To remove roles from the list, [Lock/Unlock Roles](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/locked-roles.md).

    :::

**Result:** The chosen users are added to the project.

- To revoke project membership, select the user and click **Delete**. This action deletes membership, not the user.
- To modify a user's roles in the project, delete them from the project, and then re-add them with modified roles.