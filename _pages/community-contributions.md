---
permalink: /community-contributions/
title: "Community Contributions"
layout: archive
author_profile: true
last_modified_at: 2026-08-10T15:46:43-04:00
toc: true
toc_label: "Contents"
toc_icon: "code"
classes: wide
---

<style>
/* Statistics Banner */
.stats-banner {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
  margin: 2rem 0;
  padding: 1.5rem;
  background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(17, 153, 142, 0.3);
}
.stat-item {
  text-align: center;
  padding: 1rem 2rem;
  min-width: 150px;
}
.stat-number {
  font-size: 2.5rem;
  font-weight: 700;
  color: white;
  display: block;
  line-height: 1;
}
.stat-label {
  color: rgba(255,255,255,0.9);
  font-size: 0.9rem;
  margin-top: 0.5rem;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Section Intro */
.section-intro {
  font-size: 1.1rem;
  color: #586069;
  margin-bottom: 1.5rem;
  line-height: 1.7;
}

/* Contribution Tables */
.contrib-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}
.contrib-table thead {
  background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
  color: white;
}
.contrib-table th {
  padding: 1rem 0.75rem;
  text-align: left;
  font-weight: 600;
  text-transform: uppercase;
  font-size: 0.8rem;
  letter-spacing: 0.5px;
}
.contrib-table td {
  padding: 0.875rem 0.75rem;
  border-bottom: 1px solid #e1e4e8;
  vertical-align: top;
}
.contrib-table tbody tr:hover {
  background: #f6f8fa;
}
.contrib-table tbody tr:nth-child(even) {
  background: #fafbfc;
}
.contrib-table tbody tr:nth-child(even):hover {
  background: #f1f3f5;
}
.contrib-table a {
  color: #0366d6;
  text-decoration: none;
}
.contrib-table a:hover {
  text-decoration: underline;
}
.contrib-number {
  color: #0366d6;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 600;
  display: inline-block;
  min-width: 2rem;
  text-align: center;
}
.contrib-type {
  display: inline-block;
  padding: 0.2rem 0.5rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 500;
  background: #e8f5e9;
  color: #388e3c;
}

/* Responsive */
@media (max-width: 768px) {
  .stats-banner {
    gap: 0.5rem;
  }
  .stat-item {
    padding: 0.75rem 1rem;
    min-width: 120px;
  }
  .stat-number {
    font-size: 2rem;
  }
  .contrib-table {
    font-size: 0.85rem;
  }
  .contrib-table th, .contrib-table td {
    padding: 0.5rem;
  }
}
</style>

I am an active contributor to the **Microsoft 365 & Power Platform Community (PnP)**, contributing code samples, web parts, connectors, and scripts that help developers build better solutions. My contributions have been featured in community calls and are used by developers worldwide.
{: .section-intro}

---

## Code Contributions

Open source samples, scripts, web parts, and connectors shared with the community.
{: .notice--info}

<table class="contrib-table">
   <thead>
      <tr>
         <th style="width: 50px">#</th>
         <th style="width: 280px">Type</th>
         <th>Topic</th>
         <th style="width: 130px">Date</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td><span class="contrib-number">22</span></td>
         <td>PnP Sample Script</td>
         <td>
            <a
               href="https://pnp.github.io/script-samples/spo-delete-empty-folders/README.html?tabs=pnpps"
               >Delete Empty Folders in SharePoint Document Library</a
            >
         </td>
         <td>02 September, 2025</td>
      </tr>
      <tr>
         <td><span class="contrib-number">21</span></td>
         <td>Copilot prompts</td>
         <td>
            <a
               href="https://github.com/pnp/copilot-prompts/tree/main/samples/m365-manage-pending-emails-conversations-prompt"
               >Track and manage pending communications</a
            >
         </td>
         <td>14 June, 2024</td>
      </tr>
      <tr>
         <td><span class="contrib-number">20</span></td>
         <td>Teams Dev Samples - Copilot</td>
         <td>
            <a
               href="https://github.com/pnp/teams-dev-samples/tree/main/samples/msgext-classified-listings-ts"
               >Classified Listings Copilot</a
            >
         </td>
         <td>12 June, 2024</td>
      </tr>
      <tr>
         <td><span class="contrib-number">19</span></td>
         <td>Microsoft Graph Connector</td>
         <td>
            <a
               href="https://adoption.microsoft.com/en-us/sample-solution-gallery/sample/pnp-graph-connector-json-restcountries/"
               >Ingest REST Countries content using PowerShell</a
            >
         </td>
         <td>28 November, 2023</td>
      </tr>
      <tr>
         <td><span class="contrib-number">18</span></td>
         <td>PnP Sample Script</td>
         <td>
            <a
               href="https://pnp.github.io/script-samples/teams-list-installed-apps/README.html?tabs=ps"
               >List apps within Teams in a tenant</a
            >
         </td>
         <td>23 November, 2023</td>
      </tr>
      <tr>
         <td><span class="contrib-number">17</span></td>
         <td>PnP Sample Script</td>
         <td>
            <a
               href="https://pnp.github.io/script-samples/teams-export-direct-routing-calls/README.html?tabs=ps"
               >Export Microsoft Teams Direct Routing Calls</a
            >
         </td>
         <td>26 October, 2023</td>
      </tr>
      <tr>
         <td><span class="contrib-number">16</span></td>
         <td>PnP Sample Script</td>
         <td>
            <a
               href="https://pnp.github.io/script-samples/teams-export-pstn-call-logs/README.html?tabs=ps"
               >Export Microsoft Teams PSTN Call Logs</a
            >
         </td>
         <td>26 October, 2023</td>
      </tr>
      <tr>
         <td><span class="contrib-number">15</span></td>
         <td>Teams Dev Samples - Bot</td>
         <td>
            <a
               href="https://github.com/pnp/teams-dev-samples/tree/main/samples/bot-knowledge-quest"
               >Knowledge Quest Teams Bot</a
            >
         </td>
         <td>10 July, 2023</td>
      </tr>
      <tr>
         <td><span class="contrib-number">14</span></td>
         <td>PnP Viva Connections ACE</td>
         <td>
            <a
               href="https://github.com/pnp/sp-dev-fx-aces/tree/main/samples/ImageCard-PublicHolidays"
               >Public Holidays</a
            >
         </td>
         <td>26 August, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">13</span></td>
         <td>PnP Sample Script</td>
         <td>
            <a
               href="https://pnp.github.io/script-samples/spo-install-deploy-spfx-solution/README.html?tabs=cli-m365-ps"
               >Install Solution and Deploy to the SharePoint site</a
            >
         </td>
         <td>14 June, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">12</span></td>
         <td>Power Platform Connector</td>
         <td>
            <a href="https://docs.microsoft.com/en-us/connectors/sessionizeip/"
               >Sessionize (Independent Publisher)</a
            >
         </td>
         <td>11 May, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">11</span></td>
         <td>Power Platform Connector</td>
         <td>
            <a href="https://docs.microsoft.com/en-us/connectors/udemyip/"
               >Udemy (Independent Publisher)</a
            >
         </td>
         <td>11 May, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">10</span></td>
         <td>PnP Sample Script</td>
         <td>
            <a
               href="https://pnp.github.io/script-samples/whiteboard-report-usage/README.html"
               >Export a csv report on all Whiteboards</a
            >
         </td>
         <td>07 March, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">09</span></td>
         <td>PnP Viva Connections ACE</td>
         <td>
            <a
               href="https://github.com/pnp/sp-dev-fx-aces/tree/main/samples/PrimaryTextCard-My-M365-Groups"
               >My M365 Groups</a
            >
         </td>
         <td>07 January, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">08</span></td>
         <td>PnP Teams Bot Sample</td>
         <td>
            <a
               href="https://github.com/pnp/teams-dev-samples/tree/main/samples/bot-teams-incidentmanagement"
               >MS Teams Incident Management Bot</a
            >
         </td>
         <td>12 July, 2021</td>
      </tr>
      <tr>
         <td><span class="contrib-number">07</span></td>
         <td>PnP SPFx Web Part</td>
         <td>
            <a
               href="https://github.com/pnp/sp-dev-fx-webparts/tree/master/samples/react-teams-meeting-app-questionnaire"
               >Questionnaire Teams Meeting App</a
            >
         </td>
         <td>22 March, 2021</td>
      </tr>
      <tr>
         <td><span class="contrib-number">06</span></td>
         <td>PnP SPFx Web Part</td>
         <td>
            <a
               href="https://github.com/pnp/sp-dev-fx-webparts/tree/master/samples/react-timeline"
               >Timeline</a
            >
         </td>
         <td>11 July, 2020</td>
      </tr>
      <tr>
         <td><span class="contrib-number">05</span></td>
         <td>PnP React Control</td>
         <td>
            <a
               href="https://pnp.github.io/sp-dev-fx-controls-react/controls/TreeView/"
               >TreeView Control</a
            >
         </td>
         <td>07 May, 2020</td>
      </tr>
      <tr>
         <td><span class="contrib-number">04</span></td>
         <td>PnP SPFx Web Part</td>
         <td>
            <a
               href="https://github.com/SharePoint/sp-dev-fx-webparts/tree/master/samples/react-manage-o365-groups"
               >Manage Office 365 Groups with SPFx</a
            >
         </td>
         <td>01 January, 2020</td>
      </tr>
      <tr>
         <td><span class="contrib-number">03</span></td>
         <td>PnP SPFx Web Part</td>
         <td>
            <a
               href="https://github.com/SharePoint/sp-dev-fx-webparts/tree/master/samples/react-google-fit"
               >Google API integration with SPFx</a
            >
         </td>
         <td>14 January, 2019</td>
      </tr>
      <tr>
         <td><span class="contrib-number">02</span></td>
         <td>PnP SPFx Web Part</td>
         <td>
            <a
               href="https://github.com/SharePoint/sp-dev-fx-webparts/tree/master/samples/react-adaptive-cards-image-gallery"
               >Image Gallery Web Part Built with Adaptive Cards</a
            >
         </td>
         <td>28 November, 2018</td>
      </tr>
      <tr>
         <td><span class="contrib-number">01</span></td>
         <td>PnP SPFx Web Part</td>
         <td>
            <a
               href="https://github.com/SharePoint/sp-dev-fx-webparts/tree/master/samples/react-display-hierarchy"
               >Display hierarchical information</a
            >
         </td>
         <td>15 October, 2018</td>
      </tr>
   </tbody>
</table>

---

## Community Demos

Presentations showcasing contributions during Microsoft 365 & Power Platform community calls.
{: .notice--info}

<table class="contrib-table">
   <thead>
      <tr>
         <th style="width: 50px">#</th>
         <th style="width: 280px">Community</th>
         <th>Topic</th>
         <th style="width: 130px">Date</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td><span class="contrib-number">10</span></td>
         <td>Microsoft 365 & Power Platform Community Call</td>
         <td>
            <a href="https://pnp.github.io/blog/weekly-agenda/25-06-16/"
               >Building efficient SharePoint Management with SharePoint
               Agents</a
            >
         </td>
         <td>19 June, 2025</td>
      </tr>
      <tr>
         <td><span class="contrib-number">09</span></td>
         <td>Microsoft 365 & Power Platform Community Call</td>
         <td>
            <a href="https://pnp.github.io/blog/weekly-agenda/24-08-26/"
               >Classified Listings Copilot</a
            >
         </td>
         <td>29 August, 2024</td>
      </tr>
      <tr>
         <td><span class="contrib-number">08</span></td>
         <td>Microsoft 365 & Power Platform Community Call</td>
         <td>
            <a href="https://pnp.github.io/blog/weekly-agenda/23-08-28/"
               >Knowledge Quest Teams Bot - HackTogether: Microsoft Teams Global
               Hack AI Winner</a
            >
         </td>
         <td>29 August, 2023</td>
      </tr>
      <tr>
         <td><span class="contrib-number">07</span></td>
         <td>SharePoint PnP Viva Connections and SPFx Community Call</td>
         <td>
            <a
               href="https://pnp.github.io/blog/microsoft-viva-and-spfx-community-call/2022-09-22/"
               >Building Viva Connections ACE for public holidays</a
            >
         </td>
         <td>22 September, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">06</span></td>
         <td>Power Platform & M365 Dev Community Call</td>
         <td>
            <a
               href="https://pnp.github.io/blog/microsoft-365-and-power-platform-development-community-call/2022-06-09/"
               >Building Independent Publisher Power Platform Connector for
               Sessionize</a
            >
         </td>
         <td>09 June, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">05</span></td>
         <td>SharePoint PnP Viva Connections & SPFx JS SIG Call</td>
         <td>
            <a
               href="https://techcommunity.microsoft.com/t5/microsoft-365-pnp-blog/viva-connections-amp-sharepoint-framework-community-call-7th-of/ba-p/3278956"
               >ACE - My M365 Groups</a
            >
         </td>
         <td>07 April, 2022</td>
      </tr>
      <tr>
         <td><span class="contrib-number">04</span></td>
         <td>SharePoint Framework Community Call</td>
         <td>
            <a
               href="https://techcommunity.microsoft.com/t5/microsoft-365-pnp-blog/sharepoint-framework-community-call-recording-8th-of-april-2021/ba-p/2261708"
               >Questionnaire Teams Meeting App with SPFx</a
            >
         </td>
         <td>08 April, 2021</td>
      </tr>
      <tr>
         <td><span class="contrib-number">03</span></td>
         <td>SharePoint Framework Community Call</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/microsoft-365/blogs/sharepoint-framework-community-call-recording-27th-of-february-2020/"
               >Office 365 Group Management with SPFx</a
            >
         </td>
         <td>27 February, 2020</td>
      </tr>
      <tr>
         <td><span class="contrib-number">02</span></td>
         <td>SharePoint Framework Community Call</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/sharepoint/blogs/sharepoint-framework-community-call-recording-14th-of-march-2019/"
               >Google API integration with SPFx</a
            >
         </td>
         <td>14 March, 2019</td>
      </tr>
      <tr>
         <td><span class="contrib-number">01</span></td>
         <td>SharePoint Framework Community Call</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/sharepoint/blogs/sharepoint-framework-community-call-recording-28th-of-feb-2019/"
               >Display Hierarchical Information in SPFx</a
            >
         </td>
         <td>28 February, 2019</td>
      </tr>
   </tbody>
</table>
