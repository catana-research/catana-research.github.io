---
layout: post
title: Project Development - JIRA
subtitle: Effective project planning and automation
photo: empty-subway-3416547.jpg
photo-alt: Pandas
tags: [JIRA, GitHub, Python]
category: [Projects]
---

Setting up an effective infrastructure for the tracking of project development can save the project from difficulties later down the road.

A popular project management tool is [JIRA](https://www.atlassian.com/software/jira), which offers free cloud-based tracking for a team of under 10 developers. With JIRA, work items can be quickly planned and monitored using kanban boards and roadmaps. 

JIRA can also be integrated with other applications, such as GitHub, to provide additional automation to the project.

## Link to GitHub

The process to link your GitHub projects to JIRA is simple:

- First add the [GitHub for Jira](https://catana.atlassian.net/plugins/servlet/ac/com.atlassian.jira.emcee/discover#!/discover/search?query=com.github.integration.production) application to your JIRA account.
- Then add the GitHub organisation you want to integrate. For development with multiple users this is recommended to be a GitHub team account to ensure all members of the team are included.


To link your GitHub projects to JIRA you first need to provide an OAuth access token for your GitHub personal account (for a single user) or your team account (for multiple users) as described [here](https://confluence.atlassian.com/adminjiracloud/connect-jira-cloud-to-github-814188429.html). The first step is to register JIRA as application with GitHub OAuth, followed by linking the token into your JIRA DVCS accounts by copying over the Client ID and secret provided by GitHub. Check you have `Auto Link New Repositories` and `Enable Smart Commits` ticked. The first ensures that all repositories you create with the approved account are covered, so you only need to apply the approval once. The second implements the automated JIRA workflow integration that synchronises changes between JIRA and GitHub for changes for ticket states. 

[Smart commits](https://support.atlassian.com/jira-software-cloud/docs/process-issues-with-smart-commits/) ticket