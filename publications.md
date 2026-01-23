---
layout: page
title: Research
permalink: /publications/
order: 0
---

## Publications
{% assign pubs = site.data.publications %} 
{% assign pubs_groups_year = pubs | group_by: "year" %}
{% assign current_year = "" %}

{% for year_group in pubs_groups_year %}
  {% assign year_index = forloop.index0 %}
  <h3>{{ year_group.name | default: "Additional" }}</h3>
  <ul>
    {% for pub in year_group.items %}
      <li class="pub-item">
        <div class="pub-container">
          <div class="pub-text">
            <div class="pub-text-title">{{ pub.title | strip_newlines | markdownify | replace: '<p>', '' | replace: '</p>', '' }}</div>
            <div class="pub-text-below">
              <div class="pub-text-authors">
              {% for author in pub.authors %}
                {% if author == "Thomas C Smits" or author == "Thomas Smits"%}<b>{{ author }}</b>{% else %}{{ author }}{% endif %}{% unless forloop.last %}, {% endunless %}
              {% endfor %}
              </div>
              <div class="pub-text-venue">
                {{ pub.venue }}
              </div>
              {% if pub.awards %}
                {% for award in pub.awards %}
                  <div class="pub-text-awards">
                    <img src="/assets/images/medal.svg" alt="Award" width="16" height="16" style="vertical-align: middle; margin-right: 4px;">
                    {{ award }}
                  </div>
                {% endfor %}
              {% endif %}
            </div>
            <div class="pub-text-links">
              {% if pub.url %}
                <a href="{{ pub.url }}" target="_blank" tabindex="0" class="pub-btn">Paper</a>
              {% endif %}
              {% if pub.citation %}
                <button class="pub-btn" onclick="togglePubText({{ year_index }}, {{ forloop.index0 }}, 'citation')">Citation</button>
              {% endif %}
              {% if pub.bibtex %}
                <button class="pub-btn" onclick="togglePubText({{ year_index }}, {{ forloop.index0 }}, 'bibtex')">BibTeX</button>
              {% endif %}
            </div>
            <div class="citations">
              {% if pub.citation %}
                <div id="citation-{{ year_index }}-{{ forloop.index0 }}" class="pub-text-output" style="display:none;">
                  {{ pub.citation | markdownify }}
                </div>
              {% endif %}
              {% if pub.bibtex %}
                <div id="bibtex-{{ year_index }}-{{ forloop.index0 }}" class="pub-text-output bibtex" style="display:none;">
                  {{ pub.bibtex | newline_to_br }}
                </div>
              {% endif %}
            </div>
          </div>
          {% if pub.image %}
            <div class="pub-image">
              <img src="/assets/images/publications/{{ pub.image }}.png" 
                  alt="{{ pub['image-alt'] | default: pub.title }}">
            </div>
          {% endif %}
        </div>
      </li>
    {% endfor %}
  </ul>
{% endfor %}


<script>
function togglePubText(yearIndex, pubIndex, type) {
  const types = ['citation', 'bibtex'];

  types.forEach(t => {
    const el = document.getElementById(`${t}-${yearIndex}-${pubIndex}`);
    const btn = document.querySelector(`.pub-btn[onclick="togglePubText(${yearIndex}, ${pubIndex}, '${t}')"]`);
    if (!el) return;

    if (t === type) {
      const isOpen = el.style.display === "block";
      el.style.display = isOpen ? "none" : "block";
      if (btn) btn.textContent = isOpen ? t.charAt(0).toUpperCase() + t.slice(1) : "Hide " + t.charAt(0).toUpperCase() + t.slice(1);
    } else {
      el.style.display = "none";
      const otherBtn = document.querySelector(`.pub-btn[onclick="togglePubText(${yearIndex}, ${pubIndex}, '${t}')"]`);
      if (otherBtn) otherBtn.textContent = t.charAt(0).toUpperCase() + t.slice(1);
    }
  });
}
</script>


<style>
.pub-container {
  display: grid;
  grid-template-columns: 1fr 150px;
  gap: 1rem;
  align-items: start;
}

.pub-container.right {
  grid-template-columns: 150 1fr;
}

.pub-image img {
  width: 100%;
  height: 100px;
  /* height: auto; */
  /* max-height: 100px; */
  border: 2px solid #ccc;
  border-radius: 8px;
  box-shadow: 2px 2px 6px rgba(0,0,0,0.2);
  object-fit: contain;
}

.pub-text {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 100px;
  margin-bottom: 25px;
}

.pub-text-title {
  font-weight: bold;
  border-bottom: 3px solid #ededed;
}

.pub-text-below {
  font-size: 0.8rem;
  margin-bottom: 0.3rem;
}

.pub-text-authors {
  font-style: italic;
  margin-bottom: 0.3rem;
}

.pub-text-venue {
  font-size: 0.8rem;
  color: #666;
  margin-bottom: 0.3rem;
}

.pub-text-awards {
  margin-bottom: 0.3rem;
}

.pub-btn {
  font-size: 0.8rem;
  padding: 0.3rem 0.6rem;
  margin-right: 0.3rem;
  border-radius: 5px;
  border: 1px solid #ccc;
  background-color: #f8f8f8;
  cursor: pointer;
}

.pub-btn:hover {
  background-color: #eee;
}

.pub-text-output {
  font-size: 0.8rem;
  margin-top: 0.3rem;
  margin-bottom: 0.3rem;
  padding: 0.3rem;
  border-left: 3px solid #ccc;
  background: #eee;
}

.bibtex {
  font-family: monospace;
  white-space: normal;
  word-wrap: break-word;
  max-width: 100%;
  overflow-x: auto;
  text-indent: 0;
}

</style>