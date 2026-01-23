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
  <h3>{{ year_group.name | default: "Additional" }}</h3>
  <ul>
    {% for pub in year_group.items %}
      <li class="pub-item">
        <div class="pub-container">
          <div class="pub-text">
            {{ pub.citation | markdownify }}
            {% if pub.awards %}
              {% for award in pub.awards %}
                <p>
                  <svg width="16" height="16" xmlns="http://www.w3.org/2000/svg">
                    <circle cx="8" cy="8" r="8" fill="gold"></circle>
                  </svg>
                  {{ award }}
                </p>
              {% endfor %}
            {% endif %}
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
</style>