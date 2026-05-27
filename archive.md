---
layout: default
title: Archive
permalink: /archive/
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

  .archive-page {
    max-width: 960px;
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
    margin-bottom: 18px;
  }

  .intro {
    color: #a8b0bd;
    line-height: 1.8;
    max-width: 680px;
    margin-bottom: 54px;
  }

  .post-row {
    display: grid;
    grid-template-columns: 150px minmax(0, 1fr);
    gap: 24px;
    padding: 24px 0;
    border-bottom: 1px solid rgba(255,255,255,0.12);
  }

  .date {
    color: #737b89;
    font-size: 14px;
  }

  .title a {
    color: #f3efe7;
    text-decoration: none;
    font-size: 22px;
    letter-spacing: 0;
  }

  .title a:hover {
    color: #d8b56d;
  }

  .cats {
    margin-top: 8px;
    color: #737b89;
    font-size: 13px;
  }

  @media (max-width: 680px) {
    .archive-page {
      padding: 36px 18px 72px;
    }

    h1 {
      font-size: 44px;
    }

    .post-row {
      grid-template-columns: 1fr;
      gap: 8px;
    }
  }
</style>

<div class="archive-page">
  <nav class="top-nav">
    <a href="{{ '/' | relative_url }}">← Home</a>
    <a href="{{ '/about/' | relative_url }}">About</a>
  </nav>

  <h1>Archive</h1>

  <p class="intro">
    All essays, notes, and experiments collected in one place.
  </p>

  {% for post in site.posts %}
    <div class="post-row">
      <div class="date">{{ post.date | date: "%b %d, %Y" }}</div>
      <div>
        <div class="title">
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </div>
        {% if post.categories %}
          <div class="cats">{{ post.categories | join: " / " }}</div>
        {% endif %}
      </div>
    </div>
  {% endfor %}
</div>
