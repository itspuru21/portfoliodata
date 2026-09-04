---
title: "Portfolio Blog 10 Building the Bespoke CMS - React as an Admin Panel"
description: "Developing a dedicated React application to serve as the administrative interface for creating and publishing Markdown content via the GitHub API."
date: "2026-09-03"
---

With the conceptual blueprint established in [Blog 9](#/blog/2026-09-04-portfolio-blog-9-the-crazy-idea-using-gitHub-as-a-headless-database), it was time to build the actual content creation engine. We had already reserved an `/admin` directory for this exact purpose back in [Blog 4](#/blog/2026-09-04-portfolio-blog-4-laying-the-groundwork-structuring-for-scalability). 

The goal was to build an internal tool that looked and functioned like a professional CMS (think WordPress or Ghost), but operated entirely in the browser and communicated exclusively with the GitHub REST API.

## Why React for the Admin?

For the public-facing portfolio, I was adamant about using pure, vanilla JavaScript to ensure zero latency and a lightweight build [as seen in Blog 6](#/blog/2026-09-04-portfolio-blog-6-the-interactive-static-ui-a-devops-approach-to-frontend). However, an administrative CMS is a different beast entirely. 

Building a CMS requires handling complex, dynamic state:
*   Real-time tracking of form inputs (Title, Date, Description).
*   Live previews of Markdown text.
*   Handling asynchronous API calls and loading states.
*   Managing authentication tokens securely.

Vanilla JavaScript would have turned into messy code very quickly under these requirements. Therefore, I spun up a lightweight React application strictly confined to the `/admin` path. 

![Screenshot of the React CMS UI](https://raw.githubusercontent.com/itspuru21/portfoliodata/main/images/blog10A.png)
*(Caption: The bespoke React Admin panel interface.)*

## Constructing the Payload

The core function of this React app is to take the user's input and format it into a string that our custom Markdown engine can later parse [like we built in Blog 7](#/blog/2026-09-04-portfolio-blog-7-the-markdown-engine-rendering-content-without-a-server). 

When I fill out the form and hit "Publish", a JavaScript function acts as a payload constructor. It takes the metadata state variables (Title, Description, Date) and dynamically wraps them in YAML frontmatter syntax `---`, then appends the raw Markdown body text below it. 

```javascript
// A simplified version of the payload constructor
const constructFileContent = () => {
  return `---
title: "${title}"
description: "${description}"
date: "${date}"
---

${markdownBody}`;
};
```

## The Base64 Bottleneck

Once the raw text payload is constructed, we hit a small but critical engineering hurdle. The GitHub API does not accept raw text in its `PUT` requests for file creation. It strictly requires the file content to be encoded in **Base64**.

Thankfully, browser JavaScript has a built-in `btoa()` function (Binary to ASCII) that handles this conversion. The React app takes the constructed Markdown string, passes it through `btoa()`, attaches my GitHub Personal Access Token (PAT) for authorization, and fires the `PUT` request directly to the content directory in the repository.

Within seconds, the file is committed to the repository, entirely bypassing the need for a local code editor or command-line Git operations. 

But dealing with API requests introduces a new challenge: managing rate limits and security tokens safely. In the next batch, we will continue the journey!