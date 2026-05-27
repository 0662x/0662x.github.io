---
layout: default
title: About
permalink: /about/
---

<style>
  body {
    background: linear-gradient(180deg, #0d1117 0%, #0b0f14 42%, #080b0f 100%);
    color: #f3efe7;
  }

  .site-header,
  .site-footer {
    display: none;
  }

  .page-content {
    padding: 0;
  }

  .wrapper {
    max-width: none;
    padding-left: 0;
    padding-right: 0;
  }

  .about-page {
    max-width: 900px;
    margin: 0 auto;
    padding: 48px 24px 80px;
  }

  .top-nav {
    display: flex;
    justify-content: space-between;
    margin-bottom: 72px;
    color: #a8b0bd;
    font-size: 14px;
  }

  .top-nav a {
    color: inherit;
    text-decoration: none;
  }

  .top-nav a:hover {
    color: #f3efe7;
  }

  h1 {
    font-size: 72px;
    line-height: 1;
    letter-spacing: 0;
    margin-bottom: 24px;
  }

  p {
    color: #a8b0bd;
    font-size: 18px;
    line-height: 1.9;
  }

  .card {
    margin-top: 42px;
    padding: 28px;
    border: 1px solid rgba(255,255,255,0.12);
    border-radius: 8px;
    background: rgba(255,255,255,0.055);
  }

  .card h2 {
    margin-top: 0;
    color: #f3efe7;
    letter-spacing: 0;
  }

  @media (max-width: 680px) {
    .about-page {
      padding: 36px 18px 72px;
    }

    h1 {
      font-size: 44px;
    }

    p {
      font-size: 16px;
    }
  }
</style>

<div class="about-page">
  <nav class="top-nav">
    <a href="{{ '/' | relative_url }}">← Home</a>
    <a href="{{ '/archive/' | relative_url }}">Archive</a>
  </nav>

  <h1>About</h1>

  <p>
    This is a personal research notebook for ideas about AI agents, memory systems,
    software engineering, and design.
  </p>

  <p>
    I use this site to keep technical thoughts visible, refine them over time,
    and turn repeated observations into clearer systems.
  </p>

  <div class="card">
    <h2>Current interests</h2>
    <p>
      AI agent memory, long-term personal agents, skill systems, human-computer interaction,
      software architecture, and technical writing.
    </p>
  </div>
</div>
