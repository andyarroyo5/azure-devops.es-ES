---
title: Release (Scrum) SharePoint dashboard 
titleSuffix: TFS
description: Shows how much work remains in the release and how quickly your team is working through the product backlog.
ms.prod: devops
ms.technology: devops-analytics
ms.topic: conceptual
ms.assetid: f280fa1f-a147-4b85-b233-5982a1eac4ac
ms.manager: mijacobs
ms.author: kaelli
author: KathrynEE
monikerRange: '< tfs-2018'
ms.date: 12/30/2016
---

# Release (Scrum) dashboard
[!INCLUDE [temp](../includes/tfs-sharepoint-version.md)]

The Release dashboard shows how much work remains in the release and how quickly your team is working through the product backlog. It displays the Release Burndown report, along with additional charts and quick access links.  
  
 ![Release burndown chart](media/scrum_releaseburndonw.png "Scrum_ReleaseBurndonw")  
  
 To view the dashboard, choose **Go to project portal** from the **Documents** page in Team Explorer or the home page of the web portal.  
  
 **Requirements**  
  
 See [Project portal dashboards](project-portal-dashboards.md).  
  
##  <a name="Data"></a> Data displayed in the dashboard  
 You can use the Release dashboard to understand how much progress the team is making to complete PBIs.  To learn about the web parts that are displayed on the dashboard, refer to the following illustration and table.  
  
 ![Release dashboard](media/alm_pg_scrum_releasedashbrd.png "ALM_PG_Scrum_ReleaseDashbrd")  
  
|Web Part|Data displayed|  
|--------------|--------------------|  
|![Step 1](media/procguid_1.png "ProcGuid_1")|Shows how much work remains at the start of each sprint in a release. This chart doesn't appear when the server that hosts Analysis Services for the team project is unavailable.<br /><br /> The source of the raw data is your product backlog. Each sprint that has been assigned to the team project or team appears along the horizontal axis. The vertical axis indicates the sum of all effort of all active backlog items at the start of each sprint. As the team updates the state of backlog items to Done, the effort remaining decreases. The amount of estimated effort on the vertical axis is in whatever unit that your scrum team has decided to use (for example, story points, size, or hours).<br /><br /> See also [Release Burndown](../sql-reports/release-burndown.md).|  
|![Step 2](media/procguid_2.png "ProcGuid_2")|Excel list of active PBIs.|  
|![Step 3](media/procguid_3.png "ProcGuid_3")|List of upcoming events derived from a SharePoint Web Part.<br /><br /> ![Import Events Web part](media/sharepoint_dashboard.png "SharePoint_Dashboard")|  
|![Step 4](media/procguid_4.png "ProcGuid_4")|Count of new, approved, committed, or done work items. You can open the list of work items by choosing each number.<br /><br /> ![Scrum work items &#40;Release dashboard&#41;](media/alm_pg_scrum_dshbrdworkitems.png "ALM_PG_Scrum_DshbrdWorkItems")|  
|![Step 5](media/procguid_6.png "ProcGuid_6")|List of recent builds and their status. You can view more details about a specific build by choosing it. This list is derived from a Team Web Access Web Part.<br /><br /> ![Recent Builds Web part](media/twsa_dashbuilds.png "TWSA_DashBuilds")<br /><br /> **Legend**:<br /><br /> ![Build in Progress](media/icon_buildstatus_1.gif "Icon_BuildStatus_1"): Build not started<br /><br /> ![Build Not Started](media/icon_buildstatus_2.gif "Icon_BuildStatus_2"): Build in progress<br /><br /> ![Build Succeeded](media/icon_buildstatus_3.gif "Icon_BuildStatus_3"): Build succeeded<br /><br /> ![Build Failed](media/icon_buildstatus_4.gif "Icon_BuildStatus_4"): Build failed<br /><br /> ![Build Stopped](media/icon_buildstatus_5.gif "Icon_BuildStatus_5"): Build stopped<br /><br /> ![Build Partially Succeeded](media/icon_buildstatus_6.gif "Icon_BuildStatus_6"): Build partially succeeded<br /><br /> For more information, see [Run, monitor, and manage](../../pipelines/overview.md).|  
|![Step 6](media/procguid_6a.png "ProcGuid_6a")|List of the most recent check-ins. You can view more details about a specific check-in by choosing it. This list is derived from a Team Web Access Web Part.<br /><br /> ![Recent Checkins Web part](media/twsa_dashcheckins.png "TWSA_DashCheckins")<br /><br /> For more information, see [Manage pending changes](../../repos/tfvc/develop-code-manage-pending-changes.md). **Note:**  If you use Git for version control, check-in data is not available.|  
  
## Related notes 
 [Project portal dashboards](project-portal-dashboards.md)