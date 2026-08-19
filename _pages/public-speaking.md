---
permalink: /public-speaking/
title: "Public Speaking"
layout: archive
author_profile: true
last_modified_at: 2026-08-10T15:46:43-04:00
toc: true
toc_label: "Contents"
toc_icon: "microphone-alt"
classes: wide
---

<style>
/* Statistics Banner */
.stats-banner {
  display: flex;
  flex-wrap: nowrap;
  gap: 1rem;
  justify-content: center;
  margin: 2rem 0;
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
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

/* Upcoming Events Card */
.upcoming-events {
  background: linear-gradient(135deg, #f5f7fa 0%, #e4e8ec 100%);
  border-left: 4px solid #28a745;
  border-radius: 8px;
  padding: 1.5rem;
  margin-bottom: 2rem;
}
.upcoming-events h3 {
  color: #28a745;
  margin-top: 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.event-card {
  background: white;
  border-radius: 8px;
  padding: 1.25rem;
  margin: 1rem 0;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  border: 1px solid #e1e4e8;
  transition: transform 0.2s, box-shadow 0.2s;
}
.event-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.12);
}
.event-name {
  font-weight: 600;
  color: #24292e;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
}
.event-topic {
  color: #0366d6;
  margin-bottom: 0.75rem;
}
.event-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  font-size: 0.9rem;
  color: #586069;
}
.event-meta span {
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

/* Event Tables */
.events-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}
.events-table thead {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}
.events-table th {
  padding: 1rem 0.75rem;
  text-align: left;
  font-weight: 600;
  text-transform: uppercase;
  font-size: 0.8rem;
  letter-spacing: 0.5px;
}
.events-table td {
  padding: 0.875rem 0.75rem;
  border-bottom: 1px solid #e1e4e8;
  vertical-align: top;
}
.events-table tbody tr:hover {
  background: #f6f8fa;
}
.events-table tbody tr:nth-child(even) {
  background: #fafbfc;
}
.events-table tbody tr:nth-child(even):hover {
  background: #f1f3f5;
}
.events-table a {
  color: #0366d6;
  text-decoration: none;
}
.events-table a:hover {
  text-decoration: underline;
}
.event-number {
  color: #0366d6;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 600;
  display: inline-block;
  min-width: 2rem;
  text-align: center;
}
.location-badge {
  display: inline-block;
  padding: 0.2rem 0.5rem;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 500;
}
.location-online {
  background: #e3f2fd;
  color: #1976d2;
}
.location-inperson {
  background: #e8f5e9;
  color: #388e3c;
}

/* Podcast Section */
.podcast-card {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
  padding: 1rem;
  border: 1px solid #e1e4e8;
  border-radius: 8px;
  margin-bottom: 0.75rem;
  background: white;
  transition: box-shadow 0.2s;
}
.podcast-card:hover {
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}
.podcast-number {
  background: #ff6b6b;
  color: white;
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.85rem;
  flex-shrink: 0;
}
.podcast-content {
  flex: 1;
  min-width: 200px;
}
.podcast-show {
  font-weight: 600;
  color: #24292e;
}
.podcast-topic {
  color: #586069;
  font-size: 0.9rem;
  margin-top: 0.25rem;
}
.podcast-topic a {
  color: #0366d6;
  text-decoration: none;
}
.podcast-topic a:hover {
  text-decoration: underline;
}
.podcast-date {
  color: #8a8a8a;
  font-size: 0.85rem;
  white-space: nowrap;
}

/* Organizer Section */
.organizer-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}
.organizer-card {
  background: white;
  border: 1px solid #e1e4e8;
  border-radius: 8px;
  padding: 1rem;
  transition: box-shadow 0.2s, transform 0.2s;
}
.organizer-card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transform: translateY(-2px);
}
.organizer-number {
  background: #6f42c1;
  color: white;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 600;
  margin-right: 0.5rem;
}
.organizer-name {
  color: #0366d6;
  font-weight: 500;
}
.organizer-meta {
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
  font-size: 0.85rem;
  color: #586069;
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
  .events-table {
    font-size: 0.85rem;
  }
  .events-table th, .events-table td {
    padding: 0.5rem;
  }
}
</style>

I am passionate about sharing knowledge and connecting with the tech community through speaking engagements, workshops, and conferences worldwide.
{: .section-intro}

<div class="stats-banner">
    <div class="stat-item">
    <span class="stat-number">8+</span>
    <span class="stat-label">Years Speaking</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">230+</span>
    <span class="stat-label">Speaking Sessions</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">46+</span>
    <span class="stat-label">Events Organized</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">14</span>
    <span class="stat-label">Podcast Appearances</span>
  </div>  
</div>

---

## Upcoming Events

Catch me at these upcoming events! I'd love to connect and discuss the latest in Microsoft technologies.
{: .notice--success}

<table class="events-table">
   <thead>
      <tr>
         <th>Event</th>
         <th>Topic</th>
         <th style="width: 140px">Date</th>
         <th style="width: 120px">Location</th>
      </tr>
   </thead>
   <tbody>
     <tr>
         <td>The Azure and AI Show</td>
         <td>
            <a href="https://www.linkedin.com/events/7495564058511884289/"
               >Building Smarter AI Agents with Microsoft IQ</a>
         </td>
         <td>22 August, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td>The AI-Native Workplace Summit 2026</td>
         <td>
            <a href="https://ainativeworkplace.com/"
               >Leading at the Frontier: Building an AI-Ready Organization</a
            >
         </td>
         <td>16 September, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
   </tbody>
</table>

---

## Past Speaking Engagements

A comprehensive list of all conferences, user groups, and community events where I've had the privilege to speak.
{: .notice--info}

<table class="events-table">
   <thead>
      <tr>
         <th style="width: 50px">#</th>
         <th>Event</th>
         <th>Topic</th>
         <th style="width: 140px">Date</th>
         <th style="width: 120px">Location</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td><span class="event-number">233</span></td>
         <td>Microsoft Build localhost: Gurugram</td>
         <td>
            <a
               href="https://www.5thir.com/conference/conference.aspx?cf=a2cd2dc6963349d791452d2657d81c30"
               >Building Intelligent Agents with Microsoft Foundry and IQ</a
            >
         </td>
         <td>30 June, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">232</span></td>
         <td>Global Fabric Day, Pune</td>
         <td>
            <a
               href="https://www.meetup.com/india-microsoft-fabric-user-group/events/315049974/"
               >Empowering Enterprise AI Agents with IQ and Microsoft Foundry</a
            >
         </td>
         <td>28 June, 2026</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">231</span></td>
         <td>GitHub Copilot Dev Days, Pune</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/313290776/"
               >GitHub Copilot: Your AI Companion for Every Workflow</a
            >
         </td>
         <td>16 May, 2026</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">230</span></td>
         <td>Global Microsoft Analytics Community Conference 4</td>
         <td>
            <a
               href="https://www.linkedin.com/feed/update/urn:li:activity:7457106950565253120/"
               >Designing Multi-Agent Solutions on Microsoft Foundry</a
            >
         </td>
         <td>10 May, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">229</span></td>
         <td>PowerAIThon 2026</td>
         <td>
            <a
               href="https://www.linkedin.com/feed/update/urn:li:activity:7454075143615713281/"
               >Building Multi-Agent Systems with Microsoft Foundry</a
            >
         </td>
         <td>09 May, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">228</span></td>
         <td>AgentCamp, Pune 2026 (In-Person)</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/313665954/"
               >Multi-Agent Systems with Microsoft Foundry, Copilot Studio, and
               A2A</a
            >
         </td>
         <td>25 April, 2026</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">227</span></td>
         <td>GSDC</td>
         <td>
            <a
               href="https://www.linkedin.com/posts/gsd-council_gsdc-microsoft365-aiagents-activity-7439241564025077760-GYbD/"
               >Implementing agents for Microsoft 365</a
            >
         </td>
         <td>20 March, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">226</span></td>
         <td>Global Power Platform Bootcamp 2026 – Data Analytic Group</td>
         <td>
            <a href="https://www.meetup.com/dataanalyticgroup/events/313221984/"
               >Governing AI Agents in the Power Platform</a
            >
         </td>
         <td>28 February, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">225</span></td>
         <td>Global Power Platform Bootcamp - Delhi Edition 2026</td>
         <td>Governing AI Agents in the Power Platform</td>
         <td>20 February, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">224</span></td>
         <td>M365 Con</td>
         <td>
            <a
               href="https://www.linkedin.com/pulse/review-from-models-agents-engineering-custom-engine-microsoft-bsa0e/"
               >From Models to Agents: Engineering Custom Engine Agents in
               Microsoft Foundry</a
            >
         </td>
         <td>23 January, 2026</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">223</span></td>
         <td>M365 Saturday Ahmedabad 2026</td>
         <td>
            <a href="https://www.meetup.com/m365ahmedabad/events/311912589/"
               >Supercharging SharePoint with AI: Agents, Copilot APIs, and
               Azure AI Foundry</a
            >
         </td>
         <td>03 January, 2026</td>
         <td>
            <span class="location-badge location-inperson"
               >Ahmedabad, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">222</span></td>
         <td>Pune DevCon 2025</td>
         <td>
            <a href="https://www.puneusergroup.org/events/devcon2025/"
               >Your Path to Building Agents for Microsoft 365</a
            >
         </td>
         <td>20 December, 2025</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">221</span></td>
         <td>Global Microsoft Analytics Community Conference 3</td>
         <td>
            <a
               href="https://app.powerbi.com/view?r=eyJrIjoiZWVjMTYzYjctM2FmYi00YjA2LWJhZGEtNzE0MTM0YTFhYTI2IiwidCI6ImMyY2E1ZGZkLTI3YjgtNGIxOS05ZmJhLTc2OWJmYTBkNjY2NyJ9"
               >Implementing agents with Microsoft Agent Framework</a
            >
         </td>
         <td>14 December, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">220</span></td>
         <td>Copilot Developer Camp</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/311883572/"
               >Build Custom Engine Agents using Microsoft 365 Agents SDK</a
            >
         </td>
         <td>29 November, 2025</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">219</span></td>
         <td>Dynamics User Group (DUG): New Jersey</td>
         <td>
            <a href="https://www.meetup.com/dugcentralnj/events/311737128/"
               >Microsoft 365 Agents: Build, Buy, or Extend</a
            >
         </td>
         <td>14 November, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">218</span></td>
         <td>Singapore Microsoft Azure, M365 & Power Platform</td>
         <td>
            <a href="https://www.meetup.com/mssgug/events/311370544/"
               >Building Smart Copilot Solutions with Declarative Agents for
               Microsoft 365</a
            >
         </td>
         <td>13 November, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">217</span></td>
         <td>India MCT Marathon</td>
         <td>
            <a href="https://techmilap.com/events/68c669973e27095ab1a90504"
               >Developing Agents with Microsoft 365 Agents SDK</a
            >
         </td>
         <td>11 October, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">216</span></td>
         <td>VS Code Dev Days, Pune</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/310369406/"
               >GitHub Copilot Agent mode in Action</a
            >
         </td>
         <td>27 September, 2025</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">215</span></td>
         <td>The AI-Native Workplace Summit 2025</td>
         <td>
            <a href="https://ai-nativeworkplace.sessionize.com/session/947442"
               >Designing Intelligent Copilot Solutions With Declarative Agents
               In Microsoft 365</a
            >
         </td>
         <td>24 September, 2025 at 2:30 PM IST</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">214</span></td>
         <td>VS Code Dev Days - Brighton</td>
         <td>
            <a
               href="https://www.meetup.com/devops-github-conf-brighton/events/310385643/"
               >Reimagining DevOps with GitHub Copilot Agents</a
            >
         </td>
         <td>17 September, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">213</span></td>
         <td>Global Microsoft Hindi (हिंदी) Conference</td>
         <td>
            <a
               href="https://global-microsoft-hindi-hindii-conferenc8090.sessionize.com/schedule"
               >From Idea to Agent: Developing with Microsoft 365 Agents SDK</a
            >
         </td>
         <td>13 September, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">212</span></td>
         <td>AI Maitri - Indore 2025</td>
         <td>
            <a
               href="https://www.communitydays.org/event/2025-09-06/ai-maitri-indore-2025"
               >Building Your Own Copilot: From Microsoft 365 to Custom
               Agents</a
            >
         </td>
         <td>06 September, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">211</span></td>
         <td>wedoAI 2025</td>
         <td>
            <a href="https://wedoai.ie/wall.html"
               >Develop your own agents using Semantic Kernel</a
            >
         </td>
         <td>28 August, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">210</span></td>
         <td>Data & AI Virtual Season – Vol. 2</td>
         <td>
            <a
               href="https://www.linkedin.com/events/dataanalyticgrouppresents-data-7356039395315011588/"
               >Building Agents with Microsoft 365 Agents SDK</a
            >
         </td>
         <td>23 August, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">209</span></td>
         <td>India Cloud Security Summit 2025</td>
         <td>
            <a href="https://www.indiacloudsecuritysummit.com/"
               >Simplifying Cybersecurity with Microsoft Security Copilot</a
            >
         </td>
         <td>12 July, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">208</span></td>
         <td>Canadian Cloud Summit 2025</td>
         <td>
            <a
               href="https://www.communitydays.org/event/2025-06-19/canadian-cloud-summit-june-2025#speakers"
               >Explore Declarative Agents for Microsoft 365</a
            >
         </td>
         <td>19 June, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">207</span></td>
         <td>
            Microsoft Analytics Conference: Microsoft Fabric, AI Power Platform
            Copilot
         </td>
         <td>
            <a href="https://www.linkedin.com/events/7292155773525049344/"
               >Designing Intelligent Copilot Solutions with Declarative Agents
               for Microsoft 365</a
            >
         </td>
         <td>10 May, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">206</span></td>
         <td>Global Power Platform Bootcamp 2025 - Saudi Arabia</td>
         <td>
            <a
               href="https://www.powerplatformbootcamp.com/2025/location-detail/?id=41b5ff5d-047f-ef11-a4e5-000d3a0c1307&city=Riyadh"
               >Navigating the Landscape: Building Copilot with Azure AI Foundry
               vs. Microsoft Copilot Studio</a
            >
         </td>
         <td>03 May, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">205</span></td>
         <td>Copilot Developer Camp - Pune (In-Person)</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/307106017/"
               >Custom Engine Agents - Build Agents for Microsoft 365 Copilot</a
            >
         </td>
         <td>26 April, 2025</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">204</span></td>
         <td>Global AI Bootcamp 2025 - Delhi Edition</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/a16f7592-99d2-40d4-bc7d-8615ef04f47a@c173cb52-a46e-4302-9b95-6df32c4e0544"
               >Explore the era of Declarative Agents for Microsoft 365</a
            >
         </td>
         <td>12 April, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">203</span></td>
         <td>Global AI Bootcamp 2025 - Charlotte - North Carolina (Digital)</td>
         <td>
            <a
               href="https://www.meetup.com/north-carolina-ai-innovators/events/307112581/"
               >Build your own agents with Semantic Kernel</a
            >
         </td>
         <td>05 April, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">202</span></td>
         <td>Workplace Ninjas Denmark</td>
         <td>
            <a href="https://www.meetup.com/wpninjasdk/events/306320912/"
               >Addressing security and compliance challenges in Microsoft
               Copilot</a
            >
         </td>
         <td>14 March, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">201</span></td>
         <td>TechWize Webinar</td>
         <td>
            <a href="https://www.linkedin.com/events/7297603528842186752/"
               >Enhancing Microsoft 365 with Declarative Agents for Intelligent
               Automation</a
            >
         </td>
         <td>27 February, 2025</td>
         <td><span class="location-badge location-inperson">Onlline</span></td>
      </tr>
      <tr>
         <td><span class="event-number">200</span></td>
         <td>Global Power Platform Bootcamp - Delhi Edition</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/c39c4c15-9d13-459e-9069-0eeba6924f63@c173cb52-a46e-4302-9b95-6df32c4e0544"
               >Building Copilot with Azure AI Foundry vs. Microsoft Copilot
               Studio</a
            >
         </td>
         <td>21 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">199</span></td>
         <td>Microsoft AI Summit 2025</td>
         <td>
            <a href="https://www.linkedin.com/events/7286995791464873984/"
               >Unlocking the Potential of Declarative Agents in Microsoft
               365</a
            >
         </td>
         <td>15 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">198</span></td>
         <td>
            Computing Technologies 4.0, Yeshwantrao Chavan College of
            Engineering (YCCE), Nagpur
         </td>
         <td>
            <a
               href="https://www.linkedin.com/posts/activity-7294976384689463296-iKM3"
               >Getting started with Generative AI</a
            >
         </td>
         <td>11 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">197</span></td>
         <td>7th Cloud8 virtual Summit 2025, cloudeight.ch</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/a761cdf9-3849-412a-a762-9d06bd62d63d@07867c6b-cc7e-449c-a9f4-083bac62aad8"
               >Unleashing SharePoint Insights with Microsoft 365 Copilot</a
            >
         </td>
         <td>07 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">196</span></td>
         <td>Global AI Days 2025 - Bremen</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/2bc40f34-0fea-494f-b6e8-998e141798cc@6eb12051-85ff-48fe-9901-58e38755cb03"
               >Unlocking Insights with GraphRAG: A Smarter Way to Retrieve
               Information</a
            >
         </td>
         <td>21 January, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">195</span></td>
         <td>M365 Saturday Ahmedabad 2025</td>
         <td>
            <a href="https://www.meetup.com/m365ahmedabad/events/303385888/"
               >Building Smart Copilot Solutions with Declarative Agents for
               Microsoft 365</a
            >
         </td>
         <td>04 January, 2025</td>
         <td>
            <span class="location-badge location-inperson"
               >Ahmedabad, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">194</span></td>
         <td>Pune DevCon 2024</td>
         <td>
            <a href="https://puneusergroup.org/events/devcon2024/"
               >Navigating the AI Landscape: Building Copilot with Azure AI
               Studio vs. Microsoft Copilot Studio</a
            >
         </td>
         <td>14 December, 2024</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">193</span></td>
         <td>M365 Saturday Bangalore 2024</td>
         <td>
            <a href="https://www.m365event.com/"
               >Choosing the Right Path: Building Microsoft Copilot Solutions</a
            >
         </td>
         <td>07 December, 2024</td>
         <td>
            <span class="location-badge location-inperson"
               >Bangalore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">192</span></td>
         <td>.NET Conf 2024 - Cloud Experts Group</td>
         <td>
            <a
               href="https://www.meetup.com/cloud-experts-group/events/303914241/"
               >Explore The Future of Coding with GitHub Copilot</a
            >
         </td>
         <td>23 November, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">191</span></td>
         <td>Season of AI - Season 2 - "Copilots"</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/303915867/"
               >Seasons of AI</a
            >
         </td>
         <td>16 November, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">190</span></td>
         <td>
            Microsoft Analytics Community Conference: Fabric, Copilot, and
            Purview
         </td>
         <td>
            <a
               href="https://app.powerbi.com/view?r=eyJrIjoiYjIzNjhjYTktYjg5My00YzQwLWI0MGMtOTNlNmYwZGFmOTM2IiwidCI6ImVhOGJkMWZkLWFjMzQtNGFlMi1iNDIxLTZjZmEyZmNmZjI0MyJ9"
               >Overcoming Security and Compliance Hurdles in Microsoft
               Copilot</a
            >
         </td>
         <td>14 November, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">189</span></td>
         <td>Low Code No Code Microsoft Power Platform Conference 2024</td>
         <td>
            <a
               href="https://www.communitydays.org/event/2024-11-09/low-code-no-code-microsoft-power-platform-conference-2024"
               >Enhance customer and employee experiences with Microsoft Copilot
               Studio</a
            >
         </td>
         <td>10 November, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">188</span></td>
         <td>Season of AI 2.0</td>
         <td>
            <a href="https://reskilll.com/event/soaiNanddeepNachan"
               >GitHub Copilot for TypeScript</a
            >
         </td>
         <td>06 November, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">187</span></td>
         <td>Future Forward Series by KPI Ladder</td>
         <td>
            <a
               href="https://www.linkedin.com/posts/swarnatama-ganguli_futureforwardseries-microsoftcopilot-aipoweredtool-activity-7254169795485450240-ZbwA"
               >Getting started with Microsoft Copilot</a
            >
         </td>
         <td>25 October, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">186</span></td>
         <td>Power Platform Learner to Leader</td>
         <td>
            <a href="https://www.linkedin.com/events/7247417851332960256/"
               >Build SharePoint Copilot with Microsoft Copilot Studio</a
            >
         </td>
         <td>19 October, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">185</span></td>
         <td>Technical Summit 2024 EN</td>
         <td>
            <a href="https://www.technicalsummit.de/"
               >Building Copilot for Microsoft 365 with Semantic Kernel</a
            >
         </td>
         <td>15 October, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">184</span></td>
         <td>M365 Saturday Coimbatore 2024</td>
         <td>
            <a
               href="https://www.tickettailor.com/events/m365saturdaycoimbatore/1357190"
               >Securely Unlocking SharePoint Insights with Microsoft 365
               Copilot</a
            >
         </td>
         <td>05 October, 2024</td>
         <td>
            <span class="location-badge location-inperson"
               >Coimbatore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">183</span></td>
         <td>Pune Tech Meetup by Deqode</td>
         <td>
            <a
               href="https://www.linkedin.com/feed/update/urn:li:activity:7243832275652513793/"
               >Shaping the future with Microsoft 365 Copilot</a
            >
         </td>
         <td>28 September, 2024</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">182</span></td>
         <td>Global Microsoft Hindi (हिंदी) Conference 2024</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/c9663d7d-c303-4ea8-9027-d4bddab126d3@c173cb52-a46e-4302-9b95-6df32c4e0544"
               >Addressing security and compliance challenges in Microsoft
               Copilot</a
            >
         </td>
         <td>21 September, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">181</span></td>
         <td>Multicloud4U</td>
         <td>
            <a
               href="https://www.5thir.com/eventportal/globalevents.aspx?eid=M8bkHLiWdBZfL9LmritWhzSyloBrel"
               >A Guide to Getting Started with Microsoft 365 Copilot</a
            >
         </td>
         <td>21 September, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">180</span></td>
         <td>Metaverse One 2024</td>
         <td>
            <a href="https://metaverse-one.space/"
               >Extend Copilot for Microsoft 365 with plugins</a
            >
         </td>
         <td>18 September, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">179</span></td>
         <td>Microsoft 365 Tech Community Indore August Meeting</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-365-tech-community-indore/events/302588940/"
               >Keynote: AI and Copilot in Microsoft Teams: Transforming the Way
               We Work Together</a
            >
         </td>
         <td>31 August, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">178</span></td>
         <td>aMP Pune 2024</td>
         <td>
            <a href="https://ampcommunity.online/amp-pune-24th-august-2024/"
               >Extend Copilot for Microsoft 365 with plugins</a
            >
         </td>
         <td>24 August, 2024</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">177</span></td>
         <td>Copilot Chronicles, Microsoft Reactor Bengaluru</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-reactor-bengaluru/events/302527542/"
               >Building Copilot for Microsoft 365 with Teams Toolkit</a
            >
         </td>
         <td>22 August, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">176</span></td>
         <td>Azure AI Influencers Day</td>
         <td>
            <a
               href="https://www.linkedin.com/feed/update/urn:li:activity:7223713032911077376/"
               >Harnessing the potential of AI with Azure OpenAI</a
            >
         </td>
         <td>12 August, 2024</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">175</span></td>
         <td>UST</td>
         <td>
            <a href="https://www.linkedin.com/company/ustglobal-moonraft/"
               >Develop Your Own Copilot with Azure AI Studio</a
            >
         </td>
         <td>09 August, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">174</span></td>
         <td>Power Platform Classmates Event 2024</td>
         <td>
            <a
               href="https://www.powerplatformclassmates.com/events/power-platform-classmate-2024-annual-event"
               >Build SharePoint Copilot with Microsoft Copilot Studio</a
            >
         </td>
         <td>28 July, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">173</span></td>
         <td>India Microsoft Tech Community</td>
         <td>
            <a
               href="https://www.meetup.com/india-ms-tech-community/events/302257274/"
               >Building Copilots with Semantic Kernel for Microsoft 365</a
            >
         </td>
         <td>26 July, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">172</span></td>
         <td>Season of AI</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/300607880/"
               >Getting started with Azure AI Studio</a
            >
         </td>
         <td>22 June, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">171</span></td>
         <td>Azure User Group Vadodara</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/e63083d2-cbcb-4e9c-b372-4b81f96e082d@e7479d55-ea32-48c2-814e-fe4ce8531b10"
               >Build your own Copilot with Azure AI Studio</a
            >
         </td>
         <td>15 June, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">170</span></td>
         <td>AI Chronicles: An Evening with Microsoft MVPs</td>
         <td>
            <a
               href="https://www.meetup.com/malaysia-ai-tech-hub/events/301275833/"
               >Harnessing the Power of Function Calling with Azure OpenAI
               Service</a
            >
         </td>
         <td>30 May, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">169</span></td>
         <td>Copilot Day Bootcamp</td>
         <td>
            <a href="https://lu.ma/CopilotDayDelhi"
               >Build SharePoint Copilot with Microsoft Copilot Studio</a
            >
         </td>
         <td>18 May, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">168</span></td>
         <td>Global Copilot Summit, Mumbai</td>
         <td>
            <a href="https://github.com/microsoft/global-copilot-summit#mumbai"
               >Extend Copilot for Microsoft 365 with plugins</a
            >
         </td>
         <td>11 May, 2024</td>
         <td>
            <span class="location-badge location-inperson">Mumbai, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">167</span></td>
         <td>Global AI Summer Bootcamp 2024</td>
         <td>
            <a href="https://www.linkedin.com/events/7183695553061429248/"
               >Build your own Copilot with Azure AI Studio</a
            >
         </td>
         <td>04 May, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">166</span></td>
         <td>Global Copilot Summit, Pune</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/299950033/"
               >Keynote - Microsoft 365 Copilot: The future is now!</a
            >
         </td>
         <td>20 April, 2024</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">165</span></td>
         <td>Global Azure AI/ML 2024 Germany/Karlsruhe & Singapore</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-ai-user-group-germany/events/299747039/"
               >Prompts, RAG, Fine tuning your AI models? When, why, and how?</a
            >
         </td>
         <td>19 April, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">164</span></td>
         <td>Azure Global Bootcamp Cologne 2024</td>
         <td>
            <a
               href="https://www.meetup.com/de-DE/azure-cologne-meetup/events/299880762/"
               >Prompts, RAG, Fine tuning your AI models? When, why, and how?</a
            >
         </td>
         <td>18 April, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">163</span></td>
         <td>Microsoft Reactor Bengaluru</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-reactor-bengaluru/events/299791172/"
               >MS Copilot expands with MS Graph connectors</a
            >
         </td>
         <td>12 April, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">162</span></td>
         <td>Microsoft Copilot and Viva Days, 2024</td>
         <td>
            <a href="https://www.mcandvivadays.com"
               >Explore The Future of Coding with GitHub Copilot</a
            >
         </td>
         <td>06 April, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">161</span></td>
         <td>Global AI Bootcamp 2024 Germany/Karlsruhe</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-ai-user-group-germany/events/299289328/"
               >Build RAG based Azure OpenAI Chatbot for MS Teams</a
            >
         </td>
         <td>22 March, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">160</span></td>
         <td>Global Power Platform Bootcamp 2024 Thiruvanthapuram</td>
         <td>
            <a
               href="https://www.meetup.com/india-ms-tech-community/events/298820835"
               >Elevate user experiences with Microsoft Copilot Studio</a
            >
         </td>
         <td>09 March, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">159</span></td>
         <td>Global Power Platform Bootcamp 2023 - Ryiadh, Saudi Arabia</td>
         <td>
            <a
               href="https://www.powerplatformbootcamp.com/2024/location-detail/?id=633d50c5-df7e-ee11-a81c-6045bd5d6a14&city=Ryiadh"
               >Empower your Power Platform solutions with OpenAI</a
            >
         </td>
         <td>02 March, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">158</span></td>
         <td>Global Power Platform Bootcamp 2024 - Bangalore</td>
         <td>
            <a
               href="https://www.powerplatformbootcamp.com/2024/location-detail/?id=f8ff267f-23ab-ee11-a81c-6045bd5d6a14&city=Bangalore"
               >Step into the Future: Using AI and Power Platform to boost the
               productivity</a
            >
         </td>
         <td>02 March, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">157</span></td>
         <td>Power Platform Bootcamp 2024, Delhi</td>
         <td>
            <a
               href="https://www.powerplatformbootcamp.com/2024/location-detail/?id=b71fe49b-259e-ee11-a81c-6045bd5d6a14&city=Delhi"
               >Empower your Power Platform solutions with OpenAI</a
            >
         </td>
         <td>23 February, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">156</span></td>
         <td>Teams Nation 2024</td>
         <td>
            <a href="https://teamsnation2024.sessionize.com/session/548630"
               >Bringing your own data to Microsoft Teams apps with Azure OpenAI
               and LangChain</a
            >
         </td>
         <td>21 February, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">155</span></td>
         <td>6th Cloud8 virtual Summit 2024</td>
         <td>
            <a
               href="https://events.teams.microsoft.com/event/4065b190-4a24-4ccb-946f-95823bca509f@07867c6b-cc7e-449c-a9f4-083bac62aad8"
               >Building Intelligent Bots for Microsoft Teams with Azure
               OpenAI</a
            >
         </td>
         <td>12 January, 2024</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">154</span></td>
         <td>M365 Saturday Ahmedabad 2024</td>
         <td>
            <a href="https://www.meetup.com/m365ahmedabad/events/296621704/"
               >Infuse intelligence into your Microsoft Teams apps using Azure
               OpenAI and LangChain</a
            >
         </td>
         <td>06 January, 2024</td>
         <td>
            <span class="location-badge location-inperson"
               >Ahmedabad, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">153</span></td>
         <td>PUG DevCon 2023</td>
         <td>
            <a href="https://www.meetup.com/puneusergroup/events/297498720/"
               >Integrating Azure OpenAI and LangChain for Empowering Microsoft
               Teams Apps with Your Custom Data</a
            >
         </td>
         <td>16 December, 2023</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">152</span></td>
         <td>Global AI Conference 2023</td>
         <td>
            <a
               href="https://globalai.community/events/global-ai-conference-december-2023/"
               >Bringing your own data to Microsoft Teams apps with Azure OpenAI
               and LangChain</a
            >
         </td>
         <td>12 December, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">151</span></td>
         <td>India Cloud Security Summit 2023</td>
         <td>
            <a href="https://www.indiacloudsecuritysummit.com/"
               >Navigating Legal Landscapes with Microsoft Purview eDiscovery
               Solutions</a
            >
         </td>
         <td>09 December, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">150</span></td>
         <td>M365 Saturday Bangalore 2023</td>
         <td>
            <a href="https://www.m365event.com/"
               >Build intelligent Microsoft Teams apps with Azure OpenAI</a
            >
         </td>
         <td>02 December, 2023</td>
         <td>
            <span class="location-badge location-inperson"
               >Bangalore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">149</span></td>
         <td>Generative AI Day</td>
         <td>
            <a
               href="https://www.meetup.com/dear-azure-and-ai-mumbai-india-meetup/events/295906499"
               >Bring your own data to Microsoft Teams chat bot with Azure
               OpenAI and LangChain</a
            >
         </td>
         <td>07 October, 2023</td>
         <td>
            <span class="location-badge location-inperson">Mumbai, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">148</span></td>
         <td>Microsoft AI Community</td>
         <td>
            <a
               href="https://www.meetup.com/meetup-group-ybzunvts/events/296336026/"
               >Bring your own data to Microsoft Teams chat bot with Azure
               OpenAI and LangChain</a
            >
         </td>
         <td>05 October, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">147</span></td>
         <td>Microsoft Power Platform Connect 2023</td>
         <td>
            <a
               href="https://www.eventbrite.co.uk/e/microsoft-power-platform-connect-2023-bangalore-tickets-679262009957"
               >Empower your Power Platform solutions with OpenAI</a
            >
         </td>
         <td>09 September, 2023</td>
         <td>
            <span class="location-badge location-inperson"
               >Bangalore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">146</span></td>
         <td>Software Architecture Conference 2023</td>
         <td>
            <a href="https://softwarearchitecture.live/"
               >Event driven architecture to process the M365 resource
               activities</a
            >
         </td>
         <td>25 August, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">145</span></td>
         <td>Microsoft Viva Days 2023</td>
         <td>
            <a href="https://www.microsoftvivadays.com/#speakers"
               >Building tailored employee experiences with Microsoft Viva
               Connections and SharePoint Framework</a
            >
         </td>
         <td>12 August, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">144</span></td>
         <td>Pune Tech Community</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/294778250/"
               >Unlocking AI Power: Exploring Azure OpenAI Integration with
               Power Platform</a
            >
         </td>
         <td>28 July, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">143</span></td>
         <td>STYAVA.DEV</td>
         <td>
            <a
               href="https://www.linkedin.com/feed/update/urn:li:activity:7077133077499543552/"
               >Getting started with SharePoint Framework</a
            >
         </td>
         <td>23 June, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">142</span></td>
         <td>aMS Berlin</td>
         <td>
            <a href="https://amsberlin.eu/sessions/"
               >Build your content understanding with Microsoft Syntex</a
            >
         </td>
         <td>15 June, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">141</span></td>
         <td>Microsoft Build After Party - Mumbai Microsoft 365 Edition</td>
         <td>Collaborative apps features and announcements</td>
         <td>10 June, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">140</span></td>
         <td>Microsoft Build After Party - Pune Tech Community</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/293633959/"
               >Transform Teams apps into multiplayer with Live Share</a
            >
         </td>
         <td>02 June, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">139</span></td>
         <td>Microsoft Build After Party - Pune Tech Community</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/293633951/"
               >Harness the power of AI: Extend Copilot and beyond</a
            >
         </td>
         <td>02 June, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">138</span></td>
         <td>Viva Round The World</td>
         <td>
            <a href="https://viva-round-the-world.sessionize.com/session/470079"
               >Implementing ACEs for Viva Connections dashboard with SharePoint
               Framework</a
            >
         </td>
         <td>02 June, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">137</span></td>
         <td>Microsoft Purview Days 2023</td>
         <td>
            <a href="https://www.microsoftpurviewday.com/#agenda"
               >Graph Notifications: A better way to process M365 Audit logs</a
            >
         </td>
         <td>06 May, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">136</span></td>
         <td>Microsoft Reactor Bengaluru, Teams Elite</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-reactor-bengaluru/events/292952517/"
               >Building Bots with Teams Toolkit</a
            >
         </td>
         <td>02 May, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">135</span></td>
         <td>
            All India Shri Shivaji Memorial Society (AISSMS) College of
            Engineering, Pune
         </td>
         <td>Explore Microsoft Services and Cloud</td>
         <td>27 April, 2023</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">134</span></td>
         <td>Teams 10x Spring 2023 - Virtual Summit</td>
         <td>
            <a
               href="https://teams10x-spring2023.sessionize.com/speaker/388554b5-f811-441c-96d9-1d9beb38074e"
               >Building tailored employee experiences with Microsoft Viva
               Connections and SharePoint Framework</a
            >
         </td>
         <td>13 April, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">133</span></td>
         <td>NYC Teams Tuesday</td>
         <td>
            <a href="https://www.meetup.com/NYC-Teams-Tuesday/events/290919102/"
               >Microsoft Syntex to manage your content better</a
            >
         </td>
         <td>21 March, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">132</span></td>
         <td>Experts Live India</td>
         <td>
            <a href="[https://expertslive.co.in/](https://konfhub.com/eli-2023"
               >Manage your Content Smarter with Microsoft Syntex</a
            >
         </td>
         <td>19 March, 2023</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">131</span></td>
         <td>Microsoft Reactor Bengaluru</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-reactor-bengaluru/events/291182007"
               >Implementing ACEs for Viva Connections dashboard</a
            >
         </td>
         <td>15 March, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">130</span></td>
         <td>Microsoft Reactor Bengaluru</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-reactor-bengaluru/events/291660578/"
               >Building Apps for Microsoft Teams</a
            >
         </td>
         <td>03 March, 2023</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">129</span></td>
         <td>Global Power Platform Bootcamp - Delhi</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/global-power-platform-bootcamp-2023-delhi-tickets-528092738597"
               >Building Custom Connectors for Power Platform</a
            >
         </td>
         <td>24 February, 2023</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">128</span></td>
         <td>M365 Saturday Ahmedabad 2023</td>
         <td>
            <a href="https://www.meetup.com/m365ahmedabad/events/289560209/"
               >Build your content understanding with Microsoft Syntex</a
            >
         </td>
         <td>07 January, 2023</td>
         <td>
            <span class="location-badge location-inperson"
               >Ahmedabad, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">127</span></td>
         <td>M365 Philly Virtual 2022</td>
         <td>
            <a
               href="https://www.communitydays.org/event/2022-12-15/m365-philly-virtual-2022"
               >Explore Microsoft Syntex</a
            >
         </td>
         <td>15 December, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">126</span></td>
         <td>Microsoft Ignite After Party</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/288791031/"
               >Introducing Microsoft Syntex</a
            >
         </td>
         <td>19 November, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">125</span></td>
         <td>M365 Chicago</td>
         <td>
            <a href="https://m365chicago.com/"
               >Microsoft Viva Connections - Set Up and Extend with SPFx</a
            >
         </td>
         <td>11 November, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">124</span></td>
         <td>M365 Chicago</td>
         <td>
            <a href="https://m365chicago.com/"
               >Yammer and It's Role in Microsoft Viva</a
            >
         </td>
         <td>11 November, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">123</span></td>
         <td>Global AI Developer Days, Tri-State</td>
         <td>
            <a
               href="https://communitydays.org/event/2022-11-02/global-ai-developers-days-tri-state#Speakers"
               >Add intelligence to your M365 Apps with Azure AI</a
            >
         </td>
         <td>02 November, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">122</span></td>
         <td>India Cloud Security Summit 2022</td>
         <td>
            <a href="https://www.indiacloudsecuritysummit.com/#agenda"
               >Ensuring Security and Compliance in Microsoft Teams</a
            >
         </td>
         <td>29 October, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">121</span></td>
         <td>The North American Collaboration Summit 2022</td>
         <td>
            <a href="http://www.collabsummit.org/schedule/"
               >Extending MS Teams with SharePoint Framework</a
            >
         </td>
         <td>14 October, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">120</span></td>
         <td>**Microsoft Ignite**</td>
         <td>
            <a
               href="https://learn.microsoft.com/events/ignite-2022/od19-microsoft-teams-best-practices-tips-tricks-from-our-experts"
               >Microsoft Teams Best Practices, Tips and Tricks from our
               experts</a
            >
         </td>
         <td>12 October, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">119</span></td>
         <td>Microsoft Reactor Bengaluru</td>
         <td>
            <a
               href="https://www.meetup.com/microsoft-reactor-bengaluru/events/288472697/"
               >Build modern employee experience with Microsoft Viva
               Connections</a
            >
         </td>
         <td>12 October, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">118</span></td>
         <td>Application Developers Day, Pune 2022</td>
         <td>
            <a
               href="https://www.meetup.com/pune-tech-community/events/288451237/"
               >Bot Development in an easy steps</a
            >
         </td>
         <td>08 October, 2022</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">117</span></td>
         <td>Teams10X</td>
         <td>
            <a
               href="https://teams10xfall22.sessionize.com/speaker/388554b5-f811-441c-96d9-1d9beb38074e"
               >Explore Shared Channels in MS Teams</a
            >
         </td>
         <td>29 September, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">116</span></td>
         <td>Global Microsoft Hindi (हिंदी) Event</td>
         <td>
            <a
               href="https://www.linkedin.com/events/globalmicrosoft-event-september6969912974203322369/"
               >Yammer and it's role in Microsoft Viva</a
            >
         </td>
         <td>24 September, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">115</span></td>
         <td>5th cloud8 virtual Summit 2022</td>
         <td>
            <a href="https://www.cloudeight.ch/"
               >Building apps for Teams meetings</a
            >
         </td>
         <td>23 September, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">114</span></td>
         <td>Microsoft Modern Workplace Academy</td>
         <td>
            <a href="https://msevents.microsoft.com/event?id=575473758"
               >Building tailored employee experiences with Microsoft Viva
               Connections and SharePoint Framework</a
            >
         </td>
         <td>23 September, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">113</span></td>
         <td>Gira Online Speakers LATAM 2022</td>
         <td>
            <a href="https://giraspeakerslatam-us.azurewebsites.net/"
               >Using Cognitive services to classify your content</a
            >
         </td>
         <td>08 August, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">112</span></td>
         <td>Azure DevDay Pune</td>
         <td>
            <a href="https://reskilll.com/event/DevDayPune"
               >Building Bots with Azure and consume anywhere</a
            >
         </td>
         <td>16 July, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">111</span></td>
         <td>Power Platform Classmates</td>
         <td>
            <a
               href="https://www.linkedin.com/posts/power-platform-classmates_powerplatform-microsoft-community-activity-6949383715244404736-Fnx8"
               >Canvas Apps</a
            >
         </td>
         <td>16 July, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">110</span></td>
         <td>aMS Delhi</td>
         <td>
            <a href="https://ams-delhi-2022.sessionize.com/"
               >Building Custom Connectors for Power Platform</a
            >
         </td>
         <td>25 June, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">109</span></td>
         <td>Simplifying Low Code with Azure and AI</td>
         <td>
            <a
               href="https://www.linkedin.com/posts/kasamshaikh_azure-ai-dearazure-activity-6939096377704493056-jpBj?utm_source=linkedin_share&utm_medium=member_desktop_web"
               >Using Azure APIs in Power Platform</a
            >
         </td>
         <td>25 June, 2022</td>
         <td>
            <span class="location-badge location-inperson">Mumbai, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">108</span></td>
         <td>aMS Pune</td>
         <td>
            <a href="https://ams.community/ams-pune-june-18-2022/"
               >Building apps for Teams meetings</a
            >
         </td>
         <td>18 June, 2022</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">107</span></td>
         <td>Hyderabad Power Platform User Group</td>
         <td>
            <a
               href="https://powerusers.microsoft.com/t5/Hyderabad-Power-Platform-User/Hyderabad-Power-Platform-User-Group-June-2022-Meetup/ev-p/1607831"
               >Custom Connectors in Power Platform</a
            >
         </td>
         <td>11 June, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">106</span></td>
         <td>Azure for All - Webinar Series by Dear Azure - Azure INDIA</td>
         <td>
            <a
               href="https://www.linkedin.com/events/powerappsforazurecloudprofessio6929479288953921537/about/"
               >Power Apps for Azure Cloud Professionals</a
            >
         </td>
         <td>05 June, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">105</span></td>
         <td>**Microsoft Build 2022**</td>
         <td>
            <a
               href="https://mybuild.microsoft.com/en-US/sessions/6b331ad9-d7f0-4041-bc25-1088c4b73ab6?source=sessions"
               >Device and cloud solutions strategies to reduce your climate
               impact, Humans of IT</a
            >
         </td>
         <td>25 May, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">104</span></td>
         <td>Microsoft Viva Days 2022</td>
         <td>
            <a href="https://www.microsoftvivadays.com/#agenda"
               >Microsoft Viva Connections - Set up and Extend with SPFx</a
            >
         </td>
         <td>14 May, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">103</span></td>
         <td>Microsoft 365 Virtual Marathon 2022</td>
         <td>
            <a href="https://www.m365virtualmarathon.com/"
               >Build engaging meeting experiences for MS Teams with Apps</a
            >
         </td>
         <td>05 May, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">102</span></td>
         <td>Microsoft 365 Virtual Marathon 2022</td>
         <td>
            <a href="https://www.m365virtualmarathon.com/"
               >Universal Actions for Adaptive Cards on Microsoft Teams</a
            >
         </td>
         <td>05 May, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">101</span></td>
         <td>Pune Tech Community Monthly Meetup, April 2022</td>
         <td>
            <a
               href="https://www.meetup.com/Pune-Tech-Community/events/285455477/"
               >Power Platform Custom Connector: Deep Dive</a
            >
         </td>
         <td>30 April, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">100</span></td>
         <td>Teams 10x Virtual Summit</td>
         <td>
            <a href="https://teamsdayonline.com/schedule-3"
               >Deploy Microsoft Viva Connections to MS Teams and Extend with
               SPFx</a
            >
         </td>
         <td>07 April, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">99</span></td>
         <td>Teams Nation Community Conference 2022</td>
         <td>
            <a href="https://www.teamsnation.online/schedule/"
               >Building apps for Teams meetings</a
            >
         </td>
         <td>23 March, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">98</span></td>
         <td>philly.NET Code Camp 2022</td>
         <td>
            <a
               href="https://philly-net-code-camp-2022.sessionize.com/session/319224"
               >Bring your SharePoint apps to MS Teams</a
            >
         </td>
         <td>04 March, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">97</span></td>
         <td>Canadian Cloud Summit 2022</td>
         <td>
            <a href="https://www.canadiancloudsummit.com/"
               >Microsoft Viva Connections - Set up and Extend with SPFx</a
            >
         </td>
         <td>17 February, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">96</span></td>
         <td>Teams Community Day 2022 - Cologne</td>
         <td>
            <a
               href="https://teamscommunityday-cologne-and-central.sessionize.com/session/314132"
               >Deploy Microsoft Viva Connections to MS Teams and Extend with
               SPFx</a
            >
         </td>
         <td>28 January, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">95</span></td>
         <td>Modern Workplace Conference Paris 2022</td>
         <td>
            <a href="https://modern-workplace.pro/agenda-2022/#page-content"
               >Microsoft Viva Connections - Set up and Extend with SPFx</a
            >
         </td>
         <td>26 January, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">94</span></td>
         <td>M365 Chicago</td>
         <td>
            <a href="https://m365chicago.com/sessions.html"
               >All about Microsoft Viva Connections</a
            >
         </td>
         <td>14 January, 2022</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">93</span></td>
         <td>M365 Saturday Bangalore 2021</td>
         <td>
            <a href="https://www.m365event.com/#agenda"
               >Microsoft Viva Connections - Set up and Extend with SPFx</a
            >
         </td>
         <td>11 December, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">92</span></td>
         <td>Global Microsoft Hindi Event</td>
         <td>
            <a href="https://www.globaldevcommunity.com/schedule-1"
               >Deploy Microsoft Viva to MS Teams and extend with SPFx</a
            >
         </td>
         <td>04 December, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">91</span></td>
         <td>MCT West Africa Cloud Bootcamp 2021</td>
         <td>
            <a
               href="https://mctwestafrica.com/schedule/microsoft-viva-connections-set-up-and-extend-with-spfx/"
               >Microsoft Viva Connections - Set up and Extend with SPFx</a
            >
         </td>
         <td>28 November, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">90</span></td>
         <td>Global Biz Apps Weekend by D365 Champs</td>
         <td>
            <a
               href="https://d365champions.com/html/gbaw2021_home/#eventschedule"
               >Explore Microsoft Power Platform Center of Excellence</a
            >
         </td>
         <td>20 November, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">89</span></td>
         <td>NYC Teams Tuesday - November</td>
         <td>
            <a href="https://www.meetup.com/NYC-Teams-Tuesday/events/280337056/"
               >Modern SharePoint Development</a
            >
         </td>
         <td>16 November, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">88</span></td>
         <td>aMS Germany</td>
         <td>
            <a href="https://ams.community/ams-germany-16-11-2021/"
               >Universal Actions for Adaptive Cards on Microsoft Teams</a
            >
         </td>
         <td>16 November, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">87</span></td>
         <td>Microsoft Ignite After Party</td>
         <td>
            <a
               href="https://www.meetup.com/Pune-Tech-Community/events/281519449/"
               >Microsoft Teams: Thrive with Hybrid Work</a
            >
         </td>
         <td>13 November, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">86</span></td>
         <td>
            Guest Speaker - [Panimalar Institute of Technology,
            Chennai](https://site.ieee.org/sb-pit/about-branch/)
         </td>
         <td>
            <a
               href="https://www.linkedin.com/feed/update/urn:li:activity:6855730460438474752/"
               >Introduction to Microsoft Azure and Office 365</a
            >
         </td>
         <td>19 October, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">85</span></td>
         <td>aMS Southeast Asia 2021</td>
         <td>
            <a href="https://amssea.asia/Sessions/#sz-session-272970"
               >Universal Actions for Adaptive Cards on Microsoft Teams</a
            >
         </td>
         <td>15 October, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">84</span></td>
         <td>Microsoft 365 UK (M365UK) User Group</td>
         <td>
            <a href="https://www.meetup.com/m365uk/events/280958459/"
               >Information Barriers in Office 365</a
            >
         </td>
         <td>13 October, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">83</span></td>
         <td>Azure Back to School 2021</td>
         <td>
            <a href="https://azurebacktoschool.github.io/"
               >Monitor Office 365 Environment with Azure Sentinel</a
            >
         </td>
         <td>19 September, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">82</span></td>
         <td>Cloud Summit 2021</td>
         <td>
            <a href="https://azuresummit.live/"
               >Monitor Office 365 Logs from Azure Sentinel</a
            >
         </td>
         <td>14 September, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">81</span></td>
         <td>India Cloud Security Summit, 2021</td>
         <td>
            <a href="https://www.indiacloudsecuritysummit.com/#agenda"
               >Information Barriers in MS Teams</a
            >
         </td>
         <td>28 August, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">80</span></td>
         <td>Power Platform Bootcamp India</td>
         <td>
            <a
               href="https://events.powercommunity.com/power-platform-bootcamp-india/"
               >Explore Microsoft Power Platform Center of Excellence</a
            >
         </td>
         <td>28 August, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">79</span></td>
         <td>Microsoft 365 Bootcamp 2021</td>
         <td>
            <a
               href="https://events.powercommunity.com/sessions/power-platform-and-ms-teams-better-together/"
               >Power Platform and MS Teams: Better together</a
            >
         </td>
         <td>21 August, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">78</span></td>
         <td>
            Microsoft Azure Associate Certifications - Preparation Series by
            Dear Azure - Azure INDIA
         </td>
         <td>
            <a
               href="https://www.eventbrite.com/e/microsoft-azure-associate-certifications-preparation-series-by-dear-azure-tickets-162201578287"
               >PL-100: Microsoft Power Platform App Maker</a
            >
         </td>
         <td>21 August, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">77</span></td>
         <td>Global Microsoft Hindi Bootcamp</td>
         <td>
            <a href="https://www.globalmicrosoftevents.com/schedule-1"
               >Monitor Office 365 Environment with Azure Sentinel</a
            >
         </td>
         <td>07 August, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">76</span></td>
         <td>M365 Chicago</td>
         <td>
            <a href="https://m365chicago.com/sessions.html#sz-session-254940"
               >Extend your SharePoint solutions to MS Teams</a
            >
         </td>
         <td>04 June, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">75</span></td>
         <td>3rd cloud8 virtual Summit 2021</td>
         <td>
            <a href="https://www.cloudeight.ch/?page_id=255"
               >Bring your SharePoint apps to MS Teams</a
            >
         </td>
         <td>04 June, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">74</span></td>
         <td>C# Corner CloudTech Student Conference, IMS Noida</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/cloudtech-student-conference"
               >Introduction to Azure Functions</a
            >
         </td>
         <td>29 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">73</span></td>
         <td>M365 Philly Virtual 2021</td>
         <td>
            <a
               href="https://www.spsevents.org/event/sharepoint-saturday-philly/sessions/"
               >Build MS Teams Applications with SharePoint</a
            >
         </td>
         <td>15 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">72</span></td>
         <td>Cloud Lunch and Learn Marathon 2021</td>
         <td>
            <a href="https://www.cloudlunchlearn.com/marathon"
               >Explore RBAC and PIM in M365</a
            >
         </td>
         <td>13 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">71</span></td>
         <td>Cloud Lunch and Learn Marathon 2021</td>
         <td>
            <a href="https://www.cloudlunchlearn.com/marathon"
               >Collaborate with Microsoft Teams and SharePoint</a
            >
         </td>
         <td>13 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">70</span></td>
         <td>Teams Nation</td>
         <td>
            <a href="https://teamsnation.sessionize.com/session/244819"
               >Extending MS Teams with SharePoint Framework</a
            >
         </td>
         <td>12 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">69</span></td>
         <td>Collabdays Nordics</td>
         <td>
            <a href="https://www.collabdays.org/2021-nordics/schedule/"
               >Protect Office 365 with Azure Sentinel</a
            >
         </td>
         <td>11 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">68</span></td>
         <td>Microsoft Global Hindi Bootcamp</td>
         <td>
            <a
               href="https://www.eventbrite.sg/e/free-microsoft-global-hindi-bootcamp-tickets-152673541673"
               >Bring governance and security to your M365 with RBAC and PIM</a
            >
         </td>
         <td>08 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">67</span></td>
         <td>Microsoft Power Night</td>
         <td>
            <a
               href="https://www.linkedin.com/video/live/urn:li:ugcPost:6793265021771104257/"
               >MS Teams Apps with SharePoint Framework</a
            >
         </td>
         <td>02 May, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">66</span></td>
         <td>Microsoft 365 Virtual Marathon</td>
         <td>
            <a
               href="https://www.m365virtualmarathon.com/sessions.html#sz-session-249012"
               >Monitor Office 365 Logs from Azure Sentinel</a
            >
         </td>
         <td>28 April, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">65</span></td>
         <td>Microsoft 365 Virtual Marathon</td>
         <td>
            <a
               href="https://www.m365virtualmarathon.com/sessions.html#sz-session-250765"
               >Bring your SharePoint apps to MS Teams</a
            >
         </td>
         <td>27 April, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">64</span></td>
         <td>Teams Day Online 3</td>
         <td>
            <a href="https://modernworkplacesummits.com/schedule-1"
               >Bring your SharePoint apps to MS Teams</a
            >
         </td>
         <td>07 April, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">63</span></td>
         <td>Power Platform Virual Conference</td>
         <td>
            <a
               href="https://www.2020twenty.net/power-platform-virtual-conference/"
               >Power Platform empowers Microsoft Teams</a
            >
         </td>
         <td>11 March, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">62</span></td>
         <td>A day with Microsoft Teams Development</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/a-day-with-microsoft-teams-development-tickets-141051048483"
               >Implementing Messaging Extensions for MS Teams</a
            >
         </td>
         <td>06 March, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">61</span></td>
         <td>Virtual Scottish Summit 2021</td>
         <td>
            <a
               href="https://scottishsummit.com/scottish-summit-2021-sessions#sz-session-222970"
               >Developing SharePoint Framework Solutions for the Enterprise</a
            >
         </td>
         <td>27 February, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">60</span></td>
         <td>Global Security and Compliance Community Conference</td>
         <td>
            <a href="https://microsoft365compliance.de/agenda"
               >Explore RBAC and PIM in M365</a
            >
         </td>
         <td>08 February, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">59</span></td>
         <td>Modern Workplace Conference Paris 2021</td>
         <td>
            <a href="https://mwcp21.sessionize.com/session/224864"
               >Project Oakdale, new data platform for Microsoft Teams</a
            >
         </td>
         <td>19 January, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">58</span></td>
         <td>philly.NET Code Camp 2021</td>
         <td>
            <a
               href="https://philly-net-code-camp-2021.sessionize.com/session/235468"
               >Change Notifications and Track Changes with Microsoft Graph</a
            >
         </td>
         <td>16 January, 2021</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">57</span></td>
         <td>M365 Saturday Coimbatore</td>
         <td>
            <a href="https://www.spsevents.org/event/m365coimbatore2020/"
               >Demystifying the Microsoft Lists Magic</a
            >
         </td>
         <td>19 December, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">56</span></td>
         <td>Microsoft 365 Saturday, Bangalore</td>
         <td>
            <a href="https://www.spsevents.org/event/bangaloresps2020/sessions/"
               >Introduction to Microsoft Dataverse for Teams</a
            >
         </td>
         <td>12 December, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">55</span></td>
         <td>M365 Saturday Ahmedabad 2020</td>
         <td>
            <a href="https://www.spsevents.org/event/ahmedabad2020/sessions/"
               >Dataverse for Teams - new data platform for Microsoft Teams</a
            >
         </td>
         <td>05 December, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">54</span></td>
         <td>aMS Germany</td>
         <td>
            <a href="https://ams-germany-2020.sessionize.com/session/224858"
               >SharePoint Content classification with AI capabilities</a
            >
         </td>
         <td>01 December, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">53</span></td>
         <td>Global Microsoft 365 Developer Bootcamp 2020, Hamburg, DE</td>
         <td>
            <a
               href="https://www.meetup.com/de-DE/Office-365-User-Group-Hamburg/events/271966324/"
               >Microsoft Lists: Evolving the value of SharePoint lists and
               beyond</a
            >
         </td>
         <td>28 November, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">52</span></td>
         <td>Global Microsoft 365 Developer Bootcamp 2020, Hamburg, DE</td>
         <td>
            <a
               href="https://www.meetup.com/de-DE/Office-365-User-Group-Hamburg/events/271966324/"
               >Collaborate on Microsoft Teams with Power Platform</a
            >
         </td>
         <td>28 November, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">51</span></td>
         <td>
            Global Microsoft 365 Developer Bootcamp 2020, Trivandrum, Kerala
         </td>
         <td>
            <a href="https://talklify.com/events/global-microsoft-365-bootcamp/"
               >Microsoft Lists: Evolving the value of SharePoint lists and
               beyond</a
            >
         </td>
         <td>31 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">50</span></td>
         <td>Global Microsoft 365 Developer Bootcamp 2020, Pune</td>
         <td>
            <a
               href="https://www.meetup.com/Pune-Tech-Community/events/272057386/"
               >Collect Input in Microsoft Teams with Task Modules</a
            >
         </td>
         <td>25 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">49</span></td>
         <td>Webinar, Dear Azure</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/mastering-microsoft-fundamentals-master-the-basics-virtual-event-2020-tickets-121523228247"
               >PL-900: Microsoft Power Platform Fundamentals</a
            >
         </td>
         <td>24 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">48</span></td>
         <td>aOS Southeast Asia 2020</td>
         <td>
            <a href="https://aossea.com/Home/Agenda"
               >Handling Change Notifications with Microsoft Graph</a
            >
         </td>
         <td>24 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">47</span></td>
         <td>Virtual M365 Friday Cincinnati</td>
         <td>
            <a
               href="https://www.spsevents.org/event/cincinnatisps2020/sessions/"
               >Collaborate on Microsoft Teams with Power Platform</a
            >
         </td>
         <td>23 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">46</span></td>
         <td>Virtual M365 Saturday Ottawa</td>
         <td>
            <a href="https://www.spsevents.org/event/m365ottawa/schedule/"
               >Developing SharePoint Framework Solutions for the Enterprise</a
            >
         </td>
         <td>17 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">45</span></td>
         <td>
            Global Microsoft 365 Developer Bootcamp, 2020, Hyderabad, India
         </td>
         <td>
            <a
               href="https://www.eventbrite.co.uk/e/global-microsoft-365-developer-bootcamp-2020-hyderabad-india-tickets-116282310521"
               >Explore RBAC and PIM in M365</a
            >
         </td>
         <td>17 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">44</span></td>
         <td>Webinar, Office 365 and Power Platform User group - India</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/273747075/"
               >SharePoint updates from Microsoft Ignite 2020</a
            >
         </td>
         <td>10 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">43</span></td>
         <td>Global Microsoft 365 Developer Bootcamp, 2020, Chennai</td>
         <td>
            <a href="https://www.meetup.com/XMonkeys360/events/271978500/"
               >Change Notifications and Track Changes with Microsoft Graph</a
            >
         </td>
         <td>10 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">42</span></td>
         <td>Microsoft Virtual Hub</td>
         <td>
            <a
               href="https://adoption.microsoft.com/virtual-hub/real-world-scenarios/#host-sharepoint-framework-web-part-as-microsoft-teams-messaging-extension"
               >Host SharePoint Framework web part as MS Teams messaging
               extension</a
            >
         </td>
         <td>07 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">41</span></td>
         <td>
            Global Microsoft 365 Developer Bootcamp, 2020, Dhaka, Bangladesh
         </td>
         <td>
            <a href="https://www.facebook.com/events/576061033057859/"
               >Collaborate on Microsoft Teams with Power Platform</a
            >
         </td>
         <td>03 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">40</span></td>
         <td>Teams Day Online 2</td>
         <td>
            <a href="https://modernworkplacesummits.com/session-list"
               >Collaborate on Microsoft Teams with Power Platform</a
            >
         </td>
         <td>30 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">39</span></td>
         <td>Global AI On Tour - Dhaka, Bangladesh</td>
         <td>
            <a href="https://www.facebook.com/events/331630477843707/"
               >Using AI to classify SharePoint Content with Cognitive and
               Sentiment analysis</a
            >
         </td>
         <td>19 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">38</span></td>
         <td>Microsoft 365 Saturday, Gurgaon</td>
         <td>
            <a href="https://www.spsevents.org/event/gurgaon2020/sessions/"
               >Developing SharePoint Framework Solutions for the Enterprise</a
            >
         </td>
         <td>19 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">37</span></td>
         <td>Global AI On Tour - NYC</td>
         <td>
            <a href="https://www.thetechplatform.com/global-ai-tour"
               >SharePoint Content classification with AI capabilities</a
            >
         </td>
         <td>09 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">36</span></td>
         <td>TeamsCommunityDay - The Tour</td>
         <td>
            <a href="https://www.teamscommunityday.de/en/sessions-09-09-2020-e/"
               >Collaborate on Microsoft Teams with Power Platform</a
            >
         </td>
         <td>09 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">35</span></td>
         <td>Webinar, Pune Tech Community</td>
         <td>
            <a
               href="https://www.meetup.com/Pune-Tech-Community/events/272694280/"
               >Accelerate SPFx development with PnP</a
            >
         </td>
         <td>29 August, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">34</span></td>
         <td>Global AI Community - AI Talks</td>
         <td>
            <a
               href="https://globalai.community/ai-talks/ai-talks-with-nanddeep-nachan"
               >Using AI to classify SharePoint Content with Cognitive and
               Sentiment analysis</a
            >
         </td>
         <td>20 August, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">33</span></td>
         <td>Webinar, Office 365 and Power Platform User group - India</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/272119991/"
               >Microsoft Inspire Updates for Office 365 & Power Platform</a
            >
         </td>
         <td>05 August, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">32</span></td>
         <td>Global 2020 Summer AI/ML Fest</td>
         <td>
            <a
               href="https://sessionize.com/view/lr72q3pl/GridSmart?format=Embed_Styled_Html&isDark=False"
               >Using AI to classify SharePoint Content with Cognitive and
               Sentiment analysis</a
            >
         </td>
         <td>31 July, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">31</span></td>
         <td>Microsoft Power Platform Webinar Series</td>
         <td>
            <a href="https://www.meetup.com/m365ahmedabad/events/271823451/"
               >Introduction to Power Platform</a
            >
         </td>
         <td>31 July, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">30</span></td>
         <td>Lightup Virtual Conference</td>
         <td>
            <a href="https://www.2020twenty.net/lightup/#agenda"
               >Using AI To Classify Sharepoint Content With Cognitive And
               Sentiment Analysis</a
            >
         </td>
         <td>14 July, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">29</span></td>
         <td>MS Teams Webinar Series</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/271651051/"
               >Microsoft Teams and Power Platform better together</a
            >
         </td>
         <td>11 July, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">28</span></td>
         <td>Office 365 and Power Platform - Monthly updates briefing</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/271479390/"
               >SharePoint updates (June 2020)</a
            >
         </td>
         <td>04 July, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">27</span></td>
         <td>MS Teams Webinar Series</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/271160700/"
               >Developing Task modules and adaptive cards for MS Teams</a
            >
         </td>
         <td>20 June, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">26</span></td>
         <td>C# Corner, Toronto Chapter Meetup</td>
         <td>
            <a
               href="https://www.meetup.com/C-Corner-Toronto-Chapter-Meetup/events/271155259/"
               >SharePoint updates from MS Build 2020</a
            >
         </td>
         <td>13 June, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">25</span></td>
         <td>Webinar</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/270761306/"
               >MS Build announcements for Microsoft 365 services</a
            >
         </td>
         <td>06 June, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">24</span></td>
         <td>Microsoft 365 Virtual Marathon</td>
         <td>
            <a href="http://www.m365virtualmarathon.com/sessions.html"
               >Automate MS Teams life cycle with MS Graph</a
            >
         </td>
         <td>28 May, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">23</span></td>
         <td>SPFx Deep Dive Webinar Series</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/lmdsrrybchbvb/"
               >Library Components in SPFx</a
            >
         </td>
         <td>16 May, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">22</span></td>
         <td>Global AI On Tour 2020, Ahmedabad</td>
         <td>
            <a
               href="https://www.meetup.com/SharePoint-User-Group/events/268945011/"
               >Using AI to classify SharePoint Content with Cognitive and
               Sentiment analysis</a
            >
         </td>
         <td>09 May, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">21</span></td>
         <td>C# Corner, Toronto Chapter Meetup</td>
         <td>
            <a
               href="https://www.meetup.com/C-Corner-Toronto-Chapter-Meetup/events/270297173/"
               >Use Fluent UI to build beauty with SharePoint</a
            >
         </td>
         <td>02 May, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">20</span></td>
         <td>C# Corner Global Monthly Virtual Meet: April 2020</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/c-sharp-corner-global-monthly-virtual-meet"
               >Developing SharePoint Framework Solutions for the Enterprise</a
            >
         </td>
         <td>15 April, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">19</span></td>
         <td>SPFx Webinar Series</td>
         <td>
            <a
               href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/xwxfrrybcgblb/"
               >Setting up developer environment for SPFx</a
            >
         </td>
         <td>08 April, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">18</span></td>
         <td>Power Platform 24 Conference 2020</td>
         <td>
            <a
               href="https://www.xrmvirtual.com/meeting-details/?id=950c9718-cf4d-ea11-a812-000d3a102d9a"
               >Power BI, Power Automate, and Power Apps: Connecting to data
               using the on-premises data gateway</a
            >
         </td>
         <td>26 February, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">17</span></td>
         <td>C# Corner, Pune Chapter Meet</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/typescript-azure-serverless-architecture-ms-teams-and-ms-graph"
               >Automate MS Teams life cycle with MS Graph</a
            >
         </td>
         <td>22 February, 2020</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">16</span></td>
         <td>Global Power Platform Bootcamp 2020, Pune</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/global-power-platform-bootcamp-pune-tickets-87262505503#"
               >Power BI, Power Automate, and Power Apps: Connecting to data
               using the on-premises data gateway</a
            >
         </td>
         <td>15 February, 2020</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">15</span></td>
         <td>Global M365 Developer Bootcamp 2019, Gurgaon</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/global-microsoft-365-developer-bootcamp-2019-gurgaon-tickets-67692611423"
               >Overview of MS Teams Graph API</a
            >
         </td>
         <td>07 December, 2019</td>
         <td>
            <span class="location-badge location-inperson">Gurgaon, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">14</span></td>
         <td>SPS Bangalore 2019</td>
         <td>
            <a href="https://www.spsevents.org/event/bangalore2019/sessions/"
               >Continuous integration and delivery pipelines for SPFx Solutions
               on Azure DevOps</a
            >
         </td>
         <td>30 November, 2019</td>
         <td>
            <span class="location-badge location-inperson"
               >Bangalore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">13</span></td>
         <td>Dear Azure, Azure Weekends</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/working-with-azure-devops-webinar-azureweekends-tickets-78798734135"
               >Automate Building, Testing and Deploying SharePoint Framework
               Solutions with Azure DevOps</a
            >
         </td>
         <td>24 November, 2019</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">12</span></td>
         <td>Global M365 Developer Bootcamp 2019, Hyderabad</td>
         <td>
            <a
               href="https://www.eventbrite.com/e/global-microsoft-365-developer-bootcamp-2019-hyderabad-tickets-67444740033"
               >Continuous integration and delivery pipelines for SPFx Solutions
               with Azure DevOps</a
            >
         </td>
         <td>23 November, 2019</td>
         <td>
            <span class="location-badge location-inperson"
               >Hyderabad, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">11</span></td>
         <td>SPS Ahmedabad 2019</td>
         <td>
            <a href="https://www.spsevents.org/event/ahmedabad2019/sessions/"
               >Explore Library components in SPFx</a
            >
         </td>
         <td>16 November, 2019</td>
         <td>
            <span class="location-badge location-inperson"
               >Ahmedabad, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">10</span></td>
         <td>C# Corner Pune DevCon19</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/c-sharp-corner-pune-devcon19"
               >DevOps for Office 365</a
            >
         </td>
         <td>24 August, 2019</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">09</span></td>
         <td>SPS Pune 2019</td>
         <td>
            <a href="http://www.spsevents.org/city/Pune/Pune2019/sessions"
               >DevOps with SharePoint Framework (SPFx)</a
            >
         </td>
         <td>29 June, 2019</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">08</span></td>
         <td>C# Corner, Pune Chapter Meet</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/using-sharepoint-office-365-for-digital-transformation-getting-started-with-conversational-ai"
               >SharePoint & Office 365 for Digital Transformation</a
            >
         </td>
         <td>25 May, 2019</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">07</span></td>
         <td>C# Corner Annual Conference 2019</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/article/c-sharp-corner-annual-conference-2019-official-recap/"
               >Building Powerful Intranets with Office 365</a
            >
         </td>
         <td>24 April, 2019</td>
         <td>
            <span class="location-badge location-inperson">Delhi, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">06</span></td>
         <td>Webinar, C# Corner, Pune Chapter</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/bring-cloud-closer-with-sharepoint-2019"
               >Bring Cloud Closer with SharePoint 2019</a
            >
         </td>
         <td>20 March, 2019</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">05</span></td>
         <td>Webinar, C# Corner, Pune Chapter</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/journey-to-modern-sharepoint"
               >Journey to Modern SharePoint</a
            >
         </td>
         <td>16 February, 2019</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">04</span></td>
         <td>Webinar, C# Corner, Pune Chapter</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/sharepoint-framework-development-with-react"
               >SharePoint Framework development with React</a
            >
         </td>
         <td>21 December, 2018</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">03</span></td>
         <td>Webinar, C# Corner, Pune Chapter</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/getting-started-with-sharepoint-framework-spfx"
               >Getting Started with SharePoint Framework (SPFx)</a
            >
         </td>
         <td>08 September, 2018</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">02</span></td>
         <td>C# Corner, Pune Chapter Meet</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/learn-sharepoint-office-365-azure-for-developers-and-azure-bot-service"
               >SharePoint Site Provisioning</a
            >
         </td>
         <td>28 July, 2018</td>
         <td>
            <span class="location-badge location-inperson">Pune, India</span>
         </td>
      </tr>
      <tr>
         <td><span class="event-number">01</span></td>
         <td>Webinar, C# Corner, Pune Chapter</td>
         <td>
            <a
               href="https://www.c-sharpcorner.com/events/modern-sites-in-sharepoint"
               >Modern Sites in SharePoint</a
            >
         </td>
         <td>09 June, 2018</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
   </tbody>
</table>

## Podcasts 
Featured on various podcasts discussing Microsoft technologies, community contributions, and career insights. 
{: .notice--info}

<div class="podcast-card">
   <div class="podcast-number">14</div>
   <div class="podcast-content">
      <div class="podcast-show">Microsoft Business Applications Podcast</div>
      <div class="podcast-topic">Power Platform Show with Mark Smith</div>
   </div>
   <div class="podcast-date">2 December, 2024</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">13</div>
   <div class="podcast-content">
      <div class="podcast-show">MVP Show with Mark Smith</div>
      <div class="podcast-topic">
         <a
            href="https://podcast.nz365guy.com/journey-of-a-microsoft-mvp-nanddeep-nachans-evolution-from-java-developer-to-office-365-expert-and-community-luminary/"
            >Evolution from Java Developer to Office 365 Expert and Community
            Luminary</a
         >
      </div>
   </div>
   <div class="podcast-date">17 January, 2024</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">12</div>
   <div class="podcast-content">
      <div class="podcast-show">Microsoft Business Applications Podcast</div>
      <div class="podcast-topic">
         <a href="https://podcast.nz365guy.com/518"
            >Nanddeep Nachan on the MVP Show</a
         >
      </div>
   </div>
   <div class="podcast-date">14 September, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">11</div>
   <div class="podcast-content">
      <div class="podcast-show">
         Hackathon Sample Project Interview with Garry Trinder
      </div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=h02uwVpYknE"
            >Knowledge Quest Teams Bot</a
         >
      </div>
   </div>
   <div class="podcast-date">07 September, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">10</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=M1qmo2V2oxk"
            >MVP Life: How to Balance Work and Community Contributions</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">9</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=5W5n3YdnUHI"
            >Azure DevOps for SharePoint Framework: Benefits and Challenges</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">8</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=MbWVS0X9v50"
            >Power Apps with Azure Cognitive Services: Enhance Your Apps with
            AI</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">7</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=OJNYl_RCpAI"
            >Azure Security for M365 Power Platform: Best Practices and Tips</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">6</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=pmNt-GGNpig"
            >Azure Monitor for SharePoint Framework: How to Troubleshoot and
            Optimize Your Solutions</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">5</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=dH76r5g13qo"
            >Azure Logic Apps for M365: How to Automate and Integrate Your
            Business Processes</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">4</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=ym21bePdF6A"
            >Azure Functions and Azure CDN for SharePoint Framework: How to
            Reduce Cost and Improve Scalability</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">3</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=ehjgabAFdwQ"
            >Azure Backup & Site Recovery for Power Apps: How to Design &
            Implement a Disaster Recovery Strategy</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">2</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=H9zPKJ-Z8ns"
            >Azure Test Plans & Application Insights for SharePoint Framework:
            How to Test & Validate Your Solution</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>
<div class="podcast-card">
   <div class="podcast-number">1</div>
   <div class="podcast-content">
      <div class="podcast-show">Alif Tech Talks</div>
      <div class="podcast-topic">
         <a href="https://www.youtube.com/watch?v=MY5u5d5igzQ"
            >The Future of M365 Power Platform and SharePoint Framework on
            Azure: Trends & Technologies to Watch</a
         >
      </div>
   </div>
   <div class="podcast-date">31 August, 2023</div>
</div>

## Event Organizer 
Beyond speaking, I actively organize tech community events to bring together developers and IT professionals. 
{: .notice--info}

<div class="organizer-grid">
   <div class="organizer-card">
      <span class="organizer-number">46</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/313290776/"
         class="organizer-name"
         >GitHub Copilot Dev Days, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 16 May, 2026</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">45</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/313665954/"
         class="organizer-name"
         >AgentCamp, Pune 2026 (In-Person)</a
      >
      <div class="organizer-meta">
         <span>📅 25 April, 2026</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">44</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/312712047/"
         class="organizer-name"
         >Women in Technology</a
      >
      <div class="organizer-meta">
         <span>📅 07 March, 2026</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">43</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/312712121/"
         class="organizer-name"
         >SharePoint at 25: A Legacy of Collaboration</a
      >
      <div class="organizer-meta">
         <span>📅 28 February, 2026</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">42</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/312521804/"
         class="organizer-name"
         >Global Power Platform & Agent Bootcamp, Pune 2026</a
      >
      <div class="organizer-meta">
         <span>📅 21 February, 2026</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">41</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/312521472/"
         class="organizer-name"
         >M365 Saturday, Pune 2025-26</a
      >
      <div class="organizer-meta">
         <span>📅 31 January, 2026</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">40</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/311883572/"
         class="organizer-name"
         >Copilot Developer Camp</a
      >
      <div class="organizer-meta">
         <span>📅 29 November, 2025</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">39</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/310905207/"
         class="organizer-name"
         >AgentCon Pune</a
      >
      <div class="organizer-meta">
         <span>📅 12 October, 2025</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">38</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/310369406/"
         class="organizer-name"
         >VS Code Dev Days, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 27 September, 2025</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">37</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/306720823/"
         class="organizer-name"
         >Global Azure 2025 - Pune (Virtual)</a
      >
      <div class="organizer-meta">
         <span>📅 10 May, 2025</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">36</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/307106017/"
         class="organizer-name"
         >Copilot Developer Camp - Pune (In-Person)</a
      >
      <div class="organizer-meta">
         <span>📅 26 April, 2025</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">35</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/304790781/"
         class="organizer-name"
         >Global AI Bootcamp, Pune 2025</a
      >
      <div class="organizer-meta">
         <span>📅 08 March, 2025</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">34</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/305142775/"
         class="organizer-name"
         >Global Power Platform Bootcamp, Pune 2025</a
      >
      <div class="organizer-meta">
         <span>📅 22 February, 2025</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">33</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/303604893/"
         class="organizer-name"
         >M365 Saturday, Pune 2024</a
      >
      <div class="organizer-meta">
         <span>📅 30 November, 2024</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">32</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/303345681/"
         class="organizer-name"
         >Hands-on Workshop: Building Azure AI Applications in the cloud and
         locally</a
      >
      <div class="organizer-meta">
         <span>📅 09 October, 2024</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">31</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/301592687/"
         class="organizer-name"
         >Microsoft 365 Productivity Day</a
      >
      <div class="organizer-meta">
         <span>📅 27 July, 2024</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">30</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/300607880/"
         class="organizer-name"
         >Season of AI</a
      >
      <div class="organizer-meta">
         <span>📅 22 June, 2024</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">29</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/299269909/"
         class="organizer-name"
         >Global Azure Pune 2024</a
      >
      <div class="organizer-meta">
         <span>📅 27 April, 2024</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">28</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/299950033/"
         class="organizer-name"
         >Global Copilot Summit, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 20 April, 2024</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">27</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/298708549/"
         class="organizer-name"
         >Global AI Bootcamp, Pune 2024</a
      >
      <div class="organizer-meta">
         <span>📅 16 March, 2024</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">26</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/297043002/"
         class="organizer-name"
         >Global Power Platform Bootcamp, Pune 2024</a
      >
      <div class="organizer-meta">
         <span>📅 24 February, 2024</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">16</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/298580954/"
         class="organizer-name"
         >Generative AI Day, Pune with Henk Boelman</a
      >
      <div class="organizer-meta">
         <span>📅 03 February, 2024</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">25</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/296373400/"
         class="organizer-name"
         >.NET Conf Pune 2023 (Virtual)</a
      >
      <div class="organizer-meta">
         <span>📅 12 January, 2024</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">24</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/297217496"
         class="organizer-name"
         >Global AI Conference In-Person Workshop</a
      >
      <div class="organizer-meta">
         <span>📅 12 December, 2023</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">23</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/296247574/"
         class="organizer-name"
         >M365 Saturday, Pune 2023</a
      >
      <div class="organizer-meta">
         <span>📅 28 October, 2023</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">22</span
      ><a
         href="https://www.meetup.com/dear-azure-and-ai-mumbai-india-meetup/events/295906499"
         class="organizer-name"
         >Generative AI Day</a
      >
      <div class="organizer-meta">
         <span>📅 07 October, 2023</span>
         <span>📍 Mumbai, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">21</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/294761638/"
         class="organizer-name"
         >Copilot Day</a
      >
      <div class="organizer-meta">
         <span>📅 02 September, 2023</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">20</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/291444341/"
         class="organizer-name"
         >Global Azure, Pune 2023</a
      >
      <div class="organizer-meta">
         <span>📅 13 May, 2023</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">19</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/287923053/"
         class="organizer-name"
         >Global AI Bootcamp, Pune 2023</a
      >
      <div class="organizer-meta">
         <span>📅 04 March, 2023</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">18</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/289814150/"
         class="organizer-name"
         >Global Power Platform Bootcamp, Pune 2023</a
      >
      <div class="organizer-meta">
         <span>📅 25 February, 2023</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">17</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/289618681/"
         class="organizer-name"
         >.NET Conf, Pune 2022</a
      >
      <div class="organizer-meta">
         <span>📅 17 December, 2022</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">16</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/287922909/"
         class="organizer-name"
         >Global AI Developer Days, Pune 2022</a
      >
      <div class="organizer-meta">
         <span>📅 29 October, 2022</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">15</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/288451237/"
         class="organizer-name"
         >Application Developers Day, Pune 2022</a
      >
      <div class="organizer-meta">
         <span>📅 08 October, 2022</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">14</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/287138179/"
         class="organizer-name"
         >M365 Cloud Day, Pune 2022</a
      >
      <div class="organizer-meta">
         <span>📅 06 August, 2022</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">13</span
      ><a
         href="https://www.meetup.com/pune-tech-community/events/286519676/"
         class="organizer-name"
         >aMS Pune (Hybrid)</a
      >
      <div class="organizer-meta">
         <span>📅 18 June, 2022</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">12</span
      ><a
         href="https://www.meetup.com/Pune-Tech-Community/events/285099113/"
         class="organizer-name"
         >Global Azure Bootcamp 2022, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 07 May, 2022</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">11</span
      ><a
         href="https://www.powerplatformbootcamp.com/2022/location-detail/?id=8284762c-ed4d-ec11-a3ee-281878f66d01&city=Pune%20Tech%20Community"
         class="organizer-name"
         >Global Power Platform Bootcamp 2022, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 19 February, 2022</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">10</span
      ><a
         href="https://www.meetup.com/Pune-Tech-Community/events/280810067/"
         class="organizer-name"
         >The Global AI Bootcamp 2022, Pune 2022</a
      >
      <div class="organizer-meta">
         <span>📅 15 January, 2022</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">09</span
      ><a
         href="https://globalai.community/global-ai-back-together/asia-global-ai-pune-4406/"
         class="organizer-name"
         >The Global AI Back Together, Pune 2021</a
      >
      <div class="organizer-meta">
         <span>📅 21 October, 2021</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">08</span
      ><a
         href="https://www.meetup.com/Pune-Tech-Community/events/"
         class="organizer-name"
         >First Time Speaker Series 2021</a
      >
      <div class="organizer-meta">
         <span>📅 August 2021</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">07</span
      ><a
         href="https://www.spsevents.org/event/punesps2021/"
         class="organizer-name"
         >M365 Saturday Pune 2021</a
      >
      <div class="organizer-meta">
         <span>📅 20 March, 2021</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">06</span
      ><a
         href="https://www.eventbrite.fr/e/global-power-platform-bootcamp-india-2021-tickets-132998599391"
         class="organizer-name"
         >Global Power Platform Bootcamp 2021, India</a
      >
      <div class="organizer-meta">
         <span>📅 19 & 20 February, 2021</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">05</span
      ><a
         href="https://www.meetup.com/Pune-Tech-Community/events/272057386/"
         class="organizer-name"
         >Global Microsoft 365 Developer Bootcamp 2020, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 24 & 25 October, 2020</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">04</span
      ><a
         href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/270546415/"
         class="organizer-name"
         >Global AI On Tour - 2020, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 20 June, 2020</span>
         <span>📍 Online</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">03</span
      ><a
         href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/lmdsrrybchbdb/"
         class="organizer-name"
         >SPFx Deep Dive Webinar Series</a
      >
      <div class="organizer-meta">
         <span>📅 May, 2020</span>
         <span>📍 Webinar Series</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">02</span
      ><a
         href="https://www.meetup.com/Microsoft-365-and-Power-Platform-User-group-India/events/rtfhrrybcgblb/"
         class="organizer-name"
         >Getting Started with SharePoint Framework</a
      >
      <div class="organizer-meta">
         <span>📅 April, 2020</span>
         <span>📍 Webinar Series</span>
      </div>
   </div>
   <div class="organizer-card">
      <span class="organizer-number">01</span
      ><a
         href="https://www.powerplatformbootcamp.com/location-detail/?id=ca4081e5-c11f-ea11-8454-281878f66ccc&city=Pune"
         class="organizer-name"
         >Global Power Platform Bootcamp 2020, Pune</a
      >
      <div class="organizer-meta">
         <span>📅 15 February, 2020</span>
         <span>📍 Pune, India</span>
      </div>
   </div>
</div>

## Other Appearances 
Additional appearances including proctoring, booth support, and expert sessions at major Microsoft events. 
{: .notice--info}

<table class="events-table">
   <thead>
      <tr>
         <th style="width: 50px">#</th>
         <th>Event</th>
         <th>Topic</th>
         <th style="width: 140px">Date</th>
         <th style="width: 120px">Location</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td><span class="event-number">12</span></td>
         <td>AI Genius Skills, MVP Proctoring</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/reactor/events/24599/"
               >Getting Started with Copilot for Azure to Deploy to the Cloud</a
            >
         </td>
         <td>04 March, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">11</span></td>
         <td>AI Genius Skills, MVP Proctoring</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/reactor/events/24596/"
               >Building Intelligent Multi-agent Systems</a
            >
         </td>
         <td>18 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">10</span></td>
         <td>AI Genius Skills, MVP Proctoring</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/reactor/events/24597/"
               >Getting started with AI Agents</a
            >
         </td>
         <td>11 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">09</span></td>
         <td>AI Genius Skills, MVP Proctoring</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/reactor/events/24595/"
               >Production-ready RAG with Azure AI Search</a
            >
         </td>
         <td>04 February, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">08</span></td>
         <td>AI Genius Skills, MVP Proctoring</td>
         <td>
            <a
               href="https://developer.microsoft.com/en-us/reactor/events/24594/"
               >Explore cutting-edge models: LLMs, SLMs and more</a
            >
         </td>
         <td>28 January, 2025</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">07</span></td>
         <td>Microsoft AI Tour, Bangalore 2025</td>
         <td>
            <a
               href="https://www.linkedin.com/posts/nanddeepnachan_see-you-at-the-microsoft-ai-tour-bangalore-activity-7282221924728926208-pSpG"
               >Hub Expert at Microsoft AI Foundry Booth</a
            >
         </td>
         <td>07 January, 2025</td>
         <td>
            <span class="location-badge location-inperson"
               >Bangalore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">06</span></td>
         <td>Microsoft AI Tour, Bangalore 2024</td>
         <td>
            <a
               href="https://www.linkedin.com/posts/nanddeepnachan_had-a-fantastic-time-at-the-microsoft-activity-7161658943281262596-Y6G5"
               >Hub Expert at Microsoft Copilot Studio Booth</a
            >
         </td>
         <td>08 February, 2024</td>
         <td>
            <span class="location-badge location-inperson"
               >Bangalore, India</span
            >
         </td>
      </tr>
      <tr>
         <td><span class="event-number">05</span></td>
         <td>Proctor, Global Microsoft 365 Developer Bootcamp, APAC Region</td>
         <td>
            <a href="https://microsoft365developerbootcamp.splashthat.com/"
               >Global Microsoft 365 Developer Bootcamp, APAC Region</a
            >
         </td>
         <td>14, 15 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">04</span></td>
         <td>Proctor, Global Microsoft 365 Developer Bootcamp, EMEA Region</td>
         <td>
            <a href="https://microsoft365developerbootcamp.splashthat.com/"
               >Global Microsoft 365 Developer Bootcamp, EMEA Region</a
            >
         </td>
         <td>13, 14 October, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">03</span></td>
         <td>Microsoft Ignite, Ask the Expert, SME Support</td>
         <td>
            <a href="https://myignite.microsoft.com/sessions/ATE-DB136"
               >ATE-DB136: Embrace a New Way of Work with Microsoft 365</a
            >
         </td>
         <td>24 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">02</span></td>
         <td>Microsoft Ignite, Ask the Expert, SME Support</td>
         <td>
            <a href="https://myignite.microsoft.com/sessions/ATE-DB155-R2/"
               >ATE-DB155-R2: Introducing modern admin capabilities to better
               service Microsoft 365 Apps for enterprise</a
            >
         </td>
         <td>24 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
      <tr>
         <td><span class="event-number">01</span></td>
         <td>Microsoft Ignite, Ask the Expert, SME Support</td>
         <td>
            <a href="https://myignite.microsoft.com/sessions/ATE-DB136-R1"
               >ATE-DB136-R1: Embrace a New Way of Work with Microsoft 365</a
            >
         </td>
         <td>24 September, 2020</td>
         <td><span class="location-badge location-online">Online</span></td>
      </tr>
   </tbody>
</table>

