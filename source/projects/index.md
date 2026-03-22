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
  border-radius: 28px;
  padding: 1.6rem 1.5rem 1.35rem;
  background: linear-gradient(
    180deg,
    rgba(20, 27, 40, 0.96) 0%,
    rgba(17, 23, 34, 0.98) 100%
  );
  border: 1px solid rgba(255, 255, 255, 0.06);
  box-shadow:
    0 18px 40px rgba(5, 10, 20, 0.28),
    0 6px 18px rgba(5, 10, 20, 0.18),
    inset 0 1px 0 rgba(255, 255, 255, 0.04);
  transition: transform 0.28s ease, box-shadow 0.28s ease;
  min-height: 260px;
}

.project-card::before {
  content: "";
  position: absolute;
  inset: 0;
  background:
    radial-gradient(circle at top left, rgba(139, 92, 246, 0.26), transparent 34%),
    radial-gradient(circle at top right, rgba(59, 130, 246, 0.18), transparent 30%),
    linear-gradient(180deg, rgba(99, 102, 241, 0.10), transparent 42%);
  z-index: 0;
  pointer-events: none;
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
  transform: translateY(-8px);
  box-shadow:
    0 24px 54px rgba(5, 10, 20, 0.34),
    0 10px 24px rgba(5, 10, 20, 0.22),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
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
  color: #f8fafc;
  text-decoration: none;
}

.project-title a:hover {
  text-decoration: none;
  opacity: 0.86;
}

.project-desc {
  margin: 0 0 1.3rem 0;
  color: rgba(226, 232, 240, 0.9);
  font-size: 1.02rem;
  line-height: 1.7;
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
  padding: 0.42rem 0.85rem;
  border-radius: 999px;
  font-size: 0.8rem;
  font-weight: 600;
  background: rgba(255, 255, 255, 0.05);
  color: #e2e8f0;
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(8px);
}

.project-link {
  margin-top: 1.35rem;
  display: inline-block;
  font-size: 0.96rem;
  font-weight: 600;
  color: #a5b4fc;
  text-decoration: none;
}

.project-link:hover {
  color: #c7d2fe;
  text-decoration: none;
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