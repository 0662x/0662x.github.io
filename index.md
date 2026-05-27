---
layout: default
title: Home
---

<style>
  :root {
    --bg: #0b0f14;
    --panel: rgba(255, 255, 255, 0.055);
    --panel-strong: rgba(255, 255, 255, 0.09);
    --text: #f3efe7;
    --muted: #a8b0bd;
    --soft: #737b89;
    --line: rgba(255, 255, 255, 0.12);
    --gold: #d8b56d;
    --blue: #8fb7ff;
    --green: #9ad7c2;
    --radius: 8px;
  }

  body {
    background: linear-gradient(180deg, #0d1117 0%, var(--bg) 42%, #080b0f 100%);
    color: var(--text);
  }

  .site-header,
  .site-footer {
    display: none;
  }

  .page-content {
    padding: 0;
  }

  .wrapper {
    max-width: 1180px;
    padding-left: 24px;
    padding-right: 24px;
  }

  a {
    color: inherit;
    text-decoration: none;
  }

  .home {
    min-height: 100vh;
    padding: 28px 0 56px;
  }

  .nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 92px;
  }

  .brand {
    display: flex;
    align-items: center;
    gap: 12px;
    font-weight: 700;
    letter-spacing: 0;
  }

  .brand-mark {
    width: 34px;
    height: 34px;
    border-radius: 50%;
    background:
      linear-gradient(135deg, rgba(216, 181, 109, 0.95), rgba(143, 183, 255, 0.9));
    box-shadow: 0 0 40px rgba(216, 181, 109, 0.22);
  }

  .nav-links {
    display: flex;
    gap: 22px;
    color: var(--muted);
    font-size: 14px;
  }

  .nav-links a:hover {
    color: var(--text);
  }

  .hero {
    display: grid;
    grid-template-columns: minmax(0, 1.15fr) minmax(280px, 0.85fr);
    gap: 48px;
    align-items: end;
    margin-bottom: 76px;
  }

  .eyebrow {
    color: var(--gold);
    font-size: 13px;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    margin-bottom: 18px;
  }

  .hero h1 {
    font-size: 78px;
    line-height: 0.98;
    letter-spacing: 0;
    margin: 0 0 28px;
    max-width: 850px;
  }

  .hero p {
    color: var(--muted);
    font-size: 18px;
    line-height: 1.85;
    max-width: 720px;
    margin: 0;
  }

  .hero-panel {
    border: 1px solid var(--line);
    background: linear-gradient(180deg, var(--panel-strong), var(--panel));
    border-radius: var(--radius);
    padding: 28px;
    box-shadow: 0 24px 80px rgba(0, 0, 0, 0.28);
    backdrop-filter: blur(18px);
  }

  .hero-panel h2 {
    font-size: 18px;
    margin: 0 0 12px;
  }

  .hero-panel p {
    font-size: 14px;
    line-height: 1.75;
    color: var(--muted);
    margin: 0 0 22px;
  }

  .signal-list {
    display: grid;
    gap: 12px;
  }

  .signal {
    display: flex;
    justify-content: space-between;
    gap: 16px;
    padding-top: 12px;
    border-top: 1px solid var(--line);
    font-size: 14px;
  }

  .signal span:first-child {
    color: var(--soft);
  }

  .signal span:last-child {
    color: var(--text);
  }

  .section {
    margin: 72px 0;
  }

  .section-head {
    display: flex;
    align-items: end;
    justify-content: space-between;
    gap: 24px;
    margin-bottom: 24px;
  }

  .section h2 {
    font-size: 28px;
    letter-spacing: 0;
    margin: 0;
  }

  .section-desc {
    color: var(--muted);
    max-width: 560px;
    line-height: 1.7;
    margin: 0;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 18px;
  }

  .card {
    border: 1px solid var(--line);
    background: var(--panel);
    border-radius: var(--radius);
    padding: 24px;
    min-height: 190px;
    transition: transform 0.22s ease, border-color 0.22s ease, background 0.22s ease;
  }

  .card:hover {
    transform: translateY(-4px);
    border-color: rgba(216, 181, 109, 0.38);
    background: var(--panel-strong);
  }

  .card-number {
    color: var(--gold);
    font-size: 13px;
    margin-bottom: 34px;
  }

  .card h3 {
    font-size: 20px;
    letter-spacing: 0;
    margin: 0 0 12px;
  }

  .card p {
    color: var(--muted);
    line-height: 1.75;
    margin: 0;
    font-size: 15px;
  }

  .featured {
    border: 1px solid var(--line);
    border-radius: var(--radius);
    overflow: hidden;
    background:
      linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.035));
    box-shadow: 0 28px 90px rgba(0, 0, 0, 0.25);
  }

  .featured-inner {
    display: grid;
    grid-template-columns: minmax(0, 0.95fr) minmax(0, 1.05fr);
  }

  .featured-art {
    min-height: 360px;
    padding: 34px;
    background:
      linear-gradient(135deg, rgba(216, 181, 109, 0.22), transparent 38%),
      linear-gradient(160deg, rgba(143, 183, 255, 0.18), transparent 52%),
      linear-gradient(135deg, #101720, #0c0f15);
    position: relative;
  }

  .featured-art::after {
    content: "";
    position: absolute;
    inset: 34px;
    border: 1px solid rgba(255, 255, 255, 0.16);
    border-radius: var(--radius);
  }

  .featured-label {
    position: relative;
    z-index: 1;
    color: rgba(255, 255, 255, 0.78);
    font-size: 13px;
    letter-spacing: 0.14em;
    text-transform: uppercase;
  }

  .featured-content {
    padding: 38px;
  }

  .post-meta {
    color: var(--gold);
    font-size: 13px;
    margin-bottom: 16px;
  }

  .featured-content h3 {
    font-size: 42px;
    line-height: 1.02;
    letter-spacing: 0;
    margin: 0 0 20px;
  }

  .featured-content p {
    color: var(--muted);
    line-height: 1.8;
    margin-bottom: 28px;
  }

  .button {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 11px 16px;
    border: 1px solid rgba(216, 181, 109, 0.5);
    border-radius: 999px;
    color: var(--text);
    background: rgba(216, 181, 109, 0.08);
    font-size: 14px;
  }

  .button:hover {
    background: rgba(216, 181, 109, 0.16);
  }

  .posts {
    display: grid;
    gap: 12px;
  }

  .post-row {
    display: grid;
    grid-template-columns: 140px minmax(0, 1fr) auto;
    gap: 20px;
    align-items: center;
    padding: 22px 0;
    border-bottom: 1px solid var(--line);
  }

  .post-row:hover .post-title {
    color: var(--gold);
  }

  .post-row:hover .post-title a {
    color: var(--gold);
  }

  .post-date {
    color: var(--soft);
    font-size: 13px;
  }

  .post-title {
    font-size: 20px;
    letter-spacing: 0;
    transition: color 0.2s ease;
  }

  .post-title a {
    color: inherit;
  }

  .post-category {
    color: var(--soft);
    font-size: 13px;
    text-align: right;
  }

  .language-links {
    display: inline-flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .featured-content .language-links {
    margin-bottom: 18px;
  }

  .language-pill {
    border: 1px solid var(--line);
    border-radius: 999px;
    color: var(--muted);
    font-size: 12px;
    line-height: 1;
    padding: 7px 10px;
  }

  .language-pill:hover,
  .language-pill.is-primary {
    border-color: rgba(216, 181, 109, 0.45);
    background: rgba(216, 181, 109, 0.12);
    color: var(--text);
  }

  .about {
    display: grid;
    grid-template-columns: 0.85fr 1.15fr;
    gap: 24px;
    border-top: 1px solid var(--line);
    padding-top: 42px;
  }

  .about h2 {
    font-size: 34px;
    line-height: 1.1;
    letter-spacing: 0;
    margin: 0;
  }

  .about p {
    color: var(--muted);
    line-height: 1.85;
    margin-top: 0;
  }

  .footer {
    margin-top: 82px;
    padding-top: 28px;
    border-top: 1px solid var(--line);
    display: flex;
    justify-content: space-between;
    gap: 24px;
    color: var(--soft);
    font-size: 13px;
  }

  @media (max-width: 860px) {
    .nav {
      align-items: flex-start;
      flex-direction: column;
      gap: 18px;
      margin-bottom: 56px;
    }

    .nav-links {
      flex-wrap: wrap;
      gap: 10px 16px;
    }

    .hero,
    .featured-inner,
    .about {
      grid-template-columns: 1fr;
    }

    .grid {
      grid-template-columns: 1fr;
    }

    .post-row {
      grid-template-columns: 1fr;
      gap: 8px;
    }

    .post-category {
      text-align: left;
    }

    .footer {
      flex-direction: column;
    }
  }

  @media (max-width: 640px) {
    .wrapper {
      padding-left: 18px;
      padding-right: 18px;
    }

    .home {
      padding-top: 22px;
    }

    .hero h1 {
      font-size: 44px;
    }

    .hero p {
      font-size: 16px;
    }

    .section-head {
      align-items: flex-start;
      flex-direction: column;
      gap: 10px;
    }

    .featured-art {
      min-height: 220px;
    }

    .featured-content {
      padding: 26px;
    }

    .featured-content h3 {
      font-size: 30px;
      line-height: 1.08;
    }
  }
</style>

<div class="home">
  <nav class="nav">
    <a class="brand" href="{{ '/' | relative_url }}">
      <span class="brand-mark"></span>
      <span>0662x</span>
    </a>

    <div class="nav-links">
      <a href="{{ '/archive/' | relative_url }}">Archive</a>
      <a href="#topics">Topics</a>
      <a href="{{ '/about/' | relative_url }}">About</a>
      <a href="https://github.com/0662x">GitHub</a>
    </div>
  </nav>

  <section class="hero">
    <div>
      <div class="eyebrow">Personal Research Notebook</div>
      <h1>Thinking about agents, memory, software, and design.</h1>
      <p>
        A quiet place for technical essays, engineering notes, and early ideas.
        I write about AI agents, long-term memory systems, software engineering,
        and the design of tools that become better through use.
      </p>
    </div>

    <aside class="hero-panel">
      <h2>Current focus</h2>
      <p>
        How should AI agents remember, forget, summarize, and turn repeated work
        into reusable skills?
      </p>

      <div class="signal-list">
        <div class="signal">
          <span>Theme</span>
          <span>Agent Memory</span>
        </div>
        <div class="signal">
          <span>Style</span>
          <span>Technical Essays</span>
        </div>
        <div class="signal">
          <span>Language</span>
          <span>Chinese / English</span>
        </div>
      </div>
    </aside>
  </section>

  <section class="section" id="topics">
    <div class="section-head">
      <h2>Topics</h2>
      <p class="section-desc">
        This blog is less about finished answers and more about shaping ideas clearly enough
        that they can become systems, papers, tools, or better questions.
      </p>
    </div>

    <div class="grid">
      <div class="card">
        <div class="card-number">01</div>
        <h3>AI Agent Memory</h3>
        <p>
          Long-term memory, daily notes, skill formation, retrieval, forgetting,
          and the architecture of personal agents.
        </p>
      </div>

      <div class="card">
        <div class="card-number">02</div>
        <h3>Software Engineering</h3>
        <p>
          Notes from debugging, automation, Git workflows, deployment, and building
          tools that survive real use.
        </p>
      </div>

      <div class="card">
        <div class="card-number">03</div>
        <h3>Design & Research</h3>
        <p>
          Interaction design, user experience, research framing, and how systems
          quietly shape human behavior.
        </p>
      </div>
    </div>
  </section>

  {% assign primary_posts = site.posts | where: "primary", true %}
  {% assign featured = primary_posts | first %}

  {% if featured %}
  <section class="section" id="writing">
    <div class="section-head">
      <h2>Featured Essay</h2>
      <p class="section-desc">
        The newest long-form piece from the notebook.
      </p>
    </div>

    <article class="featured">
      <div class="featured-inner">
        <div class="featured-art">
          <div class="featured-label">Featured</div>
        </div>

        <div class="featured-content">
          <div class="post-meta">
            {{ featured.date | date: "%B %d, %Y" }}
            {% if featured.categories %}
              · {{ featured.categories | join: ", " }}
            {% endif %}
          </div>

          {% assign featured_translations = site.posts | where: "translation_key", featured.translation_key %}
          {% assign featured_zh = featured_translations | where: "lang", "zh" | first %}
          {% assign featured_en = featured_translations | where: "lang", "en" | first %}
          {% if featured_translations.size > 1 %}
            <div class="language-links" aria-label="Language versions">
              {% if featured_zh %}
                <a class="language-pill is-primary" href="{{ featured_zh.url | relative_url }}">中文</a>
              {% endif %}
              {% if featured_en %}
                <a class="language-pill" href="{{ featured_en.url | relative_url }}">English</a>
              {% endif %}
            </div>
          {% endif %}

          <h3>
            <a href="{{ featured.url | relative_url }}">{{ featured.title }}</a>
          </h3>

          <p>
            {{ featured.description | default: featured.excerpt | strip_html | truncate: 180 }}
          </p>

          <a class="button" href="{{ featured.url | relative_url }}">
            Read essay →
          </a>
        </div>
      </div>
    </article>
  </section>
  {% endif %}

  <section class="section">
    <div class="section-head">
      <h2>Latest Posts</h2>
      <p class="section-desc">
        Recent writing, notes, and experiments.
      </p>
    </div>

    <div class="posts">
      {% for post in primary_posts offset: 1 limit: 8 %}
        <article class="post-row">
          <div class="post-date">{{ post.date | date: "%b %d, %Y" }}</div>
          <div class="post-title">
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          </div>
          <div class="post-category">
            {% if post.categories %}
              <div>{{ post.categories | join: " / " }}</div>
            {% endif %}
            {% assign translations = site.posts | where: "translation_key", post.translation_key %}
            {% assign zh_translation = translations | where: "lang", "zh" | first %}
            {% assign en_translation = translations | where: "lang", "en" | first %}
            {% if translations.size > 1 %}
              <div class="language-links" aria-label="Language versions">
                {% if zh_translation %}
                  <a class="language-pill is-primary" href="{{ zh_translation.url | relative_url }}">中文</a>
                {% endif %}
                {% if en_translation %}
                  <a class="language-pill" href="{{ en_translation.url | relative_url }}">English</a>
                {% endif %}
              </div>
            {% endif %}
          </div>
        </article>
      {% endfor %}
    </div>
  </section>

  <section class="section about" id="about">
    <h2>About this site</h2>
    <div>
      <p>
        This site is a personal archive for thoughts that are still forming.
        Some posts are polished essays, some are technical notes, and some are
        early explorations that may later become projects.
      </p>
      <p>
        The goal is simple: keep useful ideas visible, refine them over time,
        and turn repeated observations into clearer systems.
      </p>
    </div>
  </section>

  <footer class="footer">
    <div>© {{ site.time | date: "%Y" }} 0662x</div>
    <div>Built with GitHub Pages · Written in Markdown</div>
  </footer>
</div>
