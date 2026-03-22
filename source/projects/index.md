---
title: Projects
layout: page
---

<style>
.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.6rem;
  margin-top: 2rem;
}

.project-card {
  position: relative;
  overflow: hidden;
  border-radius: 22px;
  padding: 1.4rem 1.4rem 1.2rem;
  background: rgba(255, 255, 255, 0.78);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  border: 1px solid rgba(255, 255, 255, 0.55);
  box-shadow:
    0 10px 30px rgba(31, 41, 55, 0.08),
    0 2px 10px rgba(31, 41, 55, 0.05);
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  min-height: 220px;
}

.project-card::before {
  content: "";
  position: absolute;
  inset: 0 0 auto 0;
  height: 92px;
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.18), rgba(56, 189, 248, 0.14));
  z-index: 0;
}

.project-card:nth-child(3n+1)::before {
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.18), rgba(168, 85, 247, 0.14));
}

.project-card:nth-child(3n+2)::before {
  background: linear-gradient(135deg, rgba(16, 185, 129, 0.16), rgba(59, 130, 246, 0.14));
}

.project-card:nth-child(3n+3)::before {
  background: linear-gradient(135deg, rgba(244, 114, 182, 0.16), rgba(251, 191, 36, 0.14));
}

.project-card:hover {
  transform: translateY(-6px);
  box-shadow:
    0 18px 40px rgba(31, 41, 55, 0.12),
    0 6px 14px rgba(31, 41, 55, 0.08);
}

.project-inner {
  position: relative;
  z-index: 1;
}

.project-title {
  margin: 0 0 0.7rem 0;
  font-size: 1.22rem;
  font-weight: 700;
  line-height: 1.35;
}

.project-title a {
  color: #1f2937;
  text-decoration: none;
}

.project-title a:hover {
  text-decoration: none;
  opacity: 0.86;
}

.project-desc {
  margin: 0 0 1rem 0;
  color: #4b5563;
  font-size: 0.96rem;
  line-height: 1.72;
}

.project-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.55rem;
  margin-top: 1rem;
}

.project-tag {
  display: inline-flex;
  align-items: center;
  padding: 0.34rem 0.78rem;
  border-radius: 999px;
  font-size: 0.78rem;
  font-weight: 600;
  background: rgba(255, 255, 255, 0.72);
  color: #334155;
  border: 1px solid rgba(148, 163, 184, 0.18);
  backdrop-filter: blur(8px);
}

.project-link {
  margin-top: 1.2rem;
  display: inline-block;
  font-size: 0.9rem;
  font-weight: 600;
  color: #4f46e5;
  text-decoration: none;
}

.project-link:hover {
  text-decoration: none;
  opacity: 0.85;
}

.empty-state {
  text-align: center;
  color: #6b7280;
  padding: 2rem 0;
}

[data-user-color-scheme="dark"] .project-card {
  background: rgba(22, 28, 36, 0.72);
  border: 1px solid rgba(255, 255, 255, 0.06);
  box-shadow:
    0 10px 30px rgba(0, 0, 0, 0.28),
    0 2px 10px rgba(0, 0, 0, 0.18);
}

[data-user-color-scheme="dark"] .project-title a {
  color: #f3f4f6;
}

[data-user-color-scheme="dark"] .project-desc {
  color: #cbd5e1;
}

[data-user-color-scheme="dark"] .project-tag {
  background: rgba(255, 255, 255, 0.08);
  color: #e5e7eb;
  border-color: rgba(255, 255, 255, 0.08);
}

[data-user-color-scheme="dark"] .project-link {
  color: #a5b4fc;
}
</style>

<div class="project-grid" id="projects"></div>

<script>
  const GITHUB_USERNAME = 'halcyon-wang-es';
  const HIDDEN_REPOS = ['halcyon-wang-es.github.io'];

  const repoCategories = {
    "MiniMaxR": "⚙︎ Tool"
  };

  fetch(`https://api.github.com/users/${GITHUB_USERNAME}/repos?sort=updated&per_page=30&type=owner`)
    .then(res => res.json())
    .then(repos => {
      const container = document.getElementById('projects');

      const filtered = repos.filter(
        repo => !repo.fork && !repo.archived && !HIDDEN_REPOS.includes(repo.name)
      );

      if (!filtered.length) {
        container.innerHTML = '<p class="empty-state">No projects yet. Stay tuned!</p>';
        return;
      }

      filtered.forEach(repo => {
        const category = repoCategories[repo.name] || "⚙︎ Tool";
        const cat = `<span class="project-tag">${category}</span>`;
        const lang = repo.language ? `<span class="project-tag">${repo.language}</span>` : '';
        const desc = repo.description || 'A project in progress.';

        container.innerHTML += `
          <article class="project-card">
            <div class="project-inner">
              <h3 class="project-title">
                <a href="${repo.html_url}" target="_blank" rel="noopener">${repo.name}</a>
              </h3>
              <p class="project-desc">${desc}</p>
              <div class="project-meta">
                ${cat}
                ${lang}
              </div>
              <a class="project-link" href="${repo.html_url}" target="_blank" rel="noopener">
                View on GitHub →
              </a>
            </div>
          </article>
        `;
      });
    })
    .catch(() => {
      document.getElementById('projects').innerHTML =
        '<p class="empty-state">Failed to load projects. Please try again later.</p>';
    });
</script>