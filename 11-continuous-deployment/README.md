# Approving and deploying Terraform configuration with Azure Pipelines

## Module Overview

In this module you will create an Azure release pipeline that will deploy the configurations to a 'test' environment, wait for human approval and then deploy the configuration to a 'production' environment.

## Create Release Pipeline

Create a new release pipeline. YAML based release pipelines are in preview and do not yet support manual approvals. You will work with classic pipelines for this workshop.

**Pipelines** > **Release** > **New Pipeline**

Start with an empty job pipeline template.

![](../images/empty-job.jpg)

Name the stage `Test`.

![](../images/stage-one.jpg)

Add the deployment artifacts with the **Add** button.

![](../images/add-artifacts.jpg)

Select the build pipeline and click **Add**.

![](../images/deployment-artifacts.jpg)

Select the stage to edit the stage tasks.

![](../images/stage-tasks.jpg)

Select **Agent job** and update the **Agent pool** to use `Hosted Ubuntu 1604`.

![](../images/build-agent.jpg)

Add a **Command Line** task, give it a name of `Deploy Hello World`.

![](../images/command-line.jpg)

Copy in the following commands.

```
RELEASE=Hello-World-$(Release.EnvironmentName)

cd $(Release.PrimaryArtifactSourceAlias)/drop/modules/hello-world
terraform init
terraform plan --out plan.out -var resource_group=$RELEASE -var dns-prefix=$RELEASE
terraform apply plan.out
```

Finally, add Azure credentials to the deployment task.

Select **Variables** > **Variable groups** > **Link Variable Group** > **azure-credentials** > **Link**.

![](../images/link-variables.jpg)

Click back on **Tasks**. On the **Terraform Deploy Test**, expand **Environment Variables** and add the following variables and values. Encrypt each with the lock

| Name | Value |
|:---|:---|
| ARM_CLIENT_ID | $(ARM_CLIENT_ID) |
| ARM_CLIENT_SECRET | $(ARM_CLIENT_SECRET) |
| ARM_TENANT_ID | $(ARM_TENANT_ID) |
| ARM_SUBSCRIPTION_ID | $(ARM_SUBSCRIPTION_ID) |

When done, the task should look like this.

![](../images/task-variables.jpg)

Click **Save** > **ok**.

![](../images/save.jpg)

Click the **Create release** button > **Create** > Select the release.

![](../images/new-release.jpg)

And click the **In progress** link to see the deployment progress.

![](../images/release.jpg)

If everything went according to plan, you should have a successful release.

![](../images/release-good.jpg)


## Create a production stage with approval

Return to the release pipeline, click **Edit**, and clone the test stage.

![](../images/clone.jpg)

Name the new stage `Production` and click save.

![](../images/production.jpg)

Return to the pipeline and select **pre-deployment conditions** on the production stage.

![](../images/conditions.jpg)

Select **Pre-deployment approvals**, and add your account as an approver.

![](../images/approval.jpg)

Click **Save** and **Ok**.

![](../images/save-two.jpg)

Click **Create Release** and **Create** to start a new release and select the release link to observe the status.

![](../images/release-two.jpg)

Once the test release has successfully deployed, the production release will need to be approved. Check out the test release, and if it looks good, approve the production release using the approve button.

![](../images/approve-release.jpg)

When done, you should see both the test and production release in the Azure portal.

![](../images/both-releases.jpg)

## Bonus

These Azure Pipeline examples have not included remote state. As an additional challenge, see if you can insert the logic to store both the test and production state in an Azure backend.