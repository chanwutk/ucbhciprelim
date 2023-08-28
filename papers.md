---
title: Papers
layout: default
nav_order: 1
---
<style>
.title {
  margin-bottom: 0px !important;
}

.title-year {
  color: grey;
  font-weight: normal;
}

.tag {
  /* font-size: 12px; */
  /* color: white; */
  /* background-color: #007bff; */
  padding: 0.1rem 0.4rem;
  /* margin-left: 0.2rem; */
  margin-top: 0.3rem;
  margin-right: 0.4rem;
  border-radius: 0.2rem;
}

.paper-detail {
  margin-left: 5px;
}

.paper-entry-content {
  margin-top: 5px;
}

.paper-entry-citation {
  color: grey;
}

.decade-papers {
  margin-left: 10px;
}

.counter {
  color: grey;
}

.hidden-paper {
  display: none;
}

.shown-paper {
  display: block;
}

.category-container {
  height: 300px;
  overflow-y: auto;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.category-container::-webkit-scrollbar {
  width: 0 !important;
}

.filter-pannel {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  flex-wrap: wrap;
}

.category-entry {
  padding-right: 20px;
  cursor: pointer;
  overflow-x: hidden;
  white-space: nowrap;
}

.category-entry:hover {
  text-decoration: underline;
}

.category-entry:active {
  text-decoration: underline;
  text-underline-offset: 4px;
}

.selected-entry {
  text-decoration: underline;
}

.empty-entry {
  color: lightgray;
}

@media (max-width: 31.25rem) {
  .decade-left-header {
    display: none;
  }
}

@media (min-width: 31.25rem) {
  .decade-top-header {
    display: none;
  }
}
</style>

Official Reading Lists:
[2002](https://people.eecs.berkeley.edu/~jfc/hci-prelim-syllabus.html),
[2015](http://people.eecs.berkeley.edu/~bjoern/prelims),
[2019](https://hci.berkeley.edu/prelims/),
[2023](https://docs.google.com/document/d/1wJOSWdT2kC-03HZuhzuXuILhHKOGhtXH4JrvQmsvR4w/edit).

{% assign papers = site.papers %}

<div class="filter-pannel">
  <div class="filter-selector filter-authors">
    {% assign all_authors = "" | split: ',' %}
    {% for paper in papers %}
      {% for author in paper.authors %}
        {% unless all_authors contains author %}
          {% assign all_authors = all_authors | push: author %}
        {% endunless %}
      {% endfor %}
    {% endfor %}

    {% assign all_authors_with_count = "" | split: ',' %}
    {% for author in all_authors %}
      {% assign paper_count = 0 %}
      {% for paper in papers %}
        {% if paper.authors contains author %}
          {% assign paper_count = paper_count | plus: 1 %}
        {% endif %}
      {% endfor %}
      {% assign paper_count_0 = paper_count | modulo: 10 %}
      {% assign paper_count_1 = paper_count | divided_by: 10 | modulo: 10 %}
      {% assign paper_count_2 = paper_count | divided_by: 100 | modulo: 10 %}
      {% assign paper_count_str = paper_count_2 | append: paper_count_1 | append: paper_count_0 %}
      {% assign a = author | split: '___' | unshift: paper_count_str | join: "___" %}
      {% assign all_authors_with_count = all_authors_with_count | push: a %}
    {% endfor %}
    {% assign all_authors_with_count = all_authors_with_count | sort | reverse %}

    {% include filter-categories.html field="Author" entries=all_authors_with_count %}
  </div>
  <div class="filter-selector filter-tags">
    {% assign all_tags = "" | split: ',' %}
    {% for paper in papers %}
      {% for tag in paper.tags %}
        {% unless all_tags contains tag %}
          {% assign all_tags = all_tags | push: tag %}
        {% endunless %}
      {% endfor %}
    {% endfor %}

    {% assign all_tags_with_count = "" | split: ',' %}
    {% for tag in all_tags %}
      {% assign paper_count = 0 %}
      {% for paper in papers %}
        {% if paper.tags contains tag %}
          {% assign paper_count = paper_count | plus: 1 %}
        {% endif %}
      {% endfor %}
      {% assign paper_count_0 = paper_count | modulo: 10 %}
      {% assign paper_count_1 = paper_count | divided_by: 10 | modulo: 10 %}
      {% assign paper_count_2 = paper_count | divided_by: 100 | modulo: 10 %}
      {% assign paper_count_str = paper_count_2 | append: paper_count_1 | append: paper_count_0 %}
      {% assign t = tag | split: '___' | unshift: paper_count_str | join: "___" %}
      {% assign all_tags_with_count = all_tags_with_count | push: t %}
    {% endfor %}
    {% assign all_tags_with_count = all_tags_with_count | sort | reverse %}

    {% include filter-categories.html field="Tag" entries=all_tags_with_count %}
  </div>
  <div class="filter-selector filter-type">
    {%
      assign papers_type_grouped = papers |
      group_by: 'type' |
      sort: 'size' |
      reverse
    %}

    {% assign all_types_with_count = "" | split: ',' %}
    {% for type in papers_type_grouped %}
      {% assign paper_count_0 = type.size | modulo: 10 %}
      {% assign paper_count_1 = type.size | divided_by: 10 | modulo: 10 %}
      {% assign paper_count_2 = type.size | divided_by: 100 | modulo: 10 %}
      {% assign paper_count_str = paper_count_2 | append: paper_count_1 | append: paper_count_0 %}
      {% assign t = type.name | split: '___' | unshift: paper_count_str | join: "___" %}
      {% assign all_types_with_count = all_types_with_count | push: t %}
    {% endfor %}

    {% include filter-categories.html field="Type" entries=all_types_with_count %}
  </div>
</div>

{%
  assign papers_grouped = papers |
  group_by_exp: 'd', 'd.year  | truncate: 3, ""' |
  sort: 'name'
%}
{% for paper_grouped in papers_grouped %}
  <div id="{{paper_grouped.name}}0s" class="d-flex decade-container">
    <h2 class="decade-left-header">{{paper_grouped.name}}0s</h2>
    <div class="decade-papers">
      <h1 class="decade-top-header">{{paper_grouped.name}}0s</h1>
      {% assign items = paper_grouped.items | sort: 'year' %}
      {% for paper in items %}
      <div id="{{paper.name | split: '.' | first}}">
        <h2 class="title">
          <span class="title-year">{{paper.year}}</span>
          {{paper.title}}
        </h2>
        <div class="paper-detail">
          {{paper.authors[0]}}
          {% for author in paper.authors offset:1 %}
            <span style='color: lightgray'>&#9679;</span> {{author}}
          {% endfor %}
          <div class="d-flex flex-wrap">
            {% for tag in paper.tags %}
            <div class="tag btn-primary">{{ tag }}</div>
            {% endfor %}
          </div>
          <div class="paper-entry-content">
            {% assign idx = 0 %}
            {% for link in paper.links %}
              {% if idx > 0 %}
                &ensp;|&ensp;
              {% endif %}
              <a href="{{link}}">{{paper.link_descriptions[idx]}}</a>
              {% assign idx = idx | plus: 1 %}
            {% endfor %}
            &ensp;
            <span class="paper-entry-citation">{{paper.citation}}</span>
          </div>
        </div>
      </div>
      {% endfor %}
    </div>
  </div>
{% endfor %}

Tags are modified from [Samantha Robertson](https://www.samantha-robertson.com/)'s Notes.

<script>
const _papers = [
  {% for paper in papers %}
    {
      title: '{{ paper.title }}',
      year: '{{ paper.year }}',
      authors: '{{ paper.authors | join: '___' }}'.split('___'),
      id: '{{ paper.name }}'.split('.')[0],
      type: '{{ paper.type }}',
      tags: '{{ paper.tags | join : '___' }}'.split('___'),
      content: '{{ paper.content }}',
    },
  {% endfor %}
];
const _selected = { authors: [], tags: [], type: null };
</script>

<script>
function hide (element) {
  element.classList.add('hidden-paper');
  element.classList.remove('shown-paper');
}

function show (element) {
  element.classList.add('shown-paper');
  element.classList.remove('hidden-paper');
}

function update (papers, selected) {
  const filtered = papers.filter(paper => {
    if ((selected.authors || []).length) {
      if (selected.authors.some(author => !paper.authors.includes(author))) {
        return false;
      }
    }
    if ((selected.tags || []).length) {
      if (selected.tags.some(tag => !paper.tags.includes(tag))) {
        return false;
      }
    }
    if (selected.type && selected.type !== paper.type) {
      return false;
    }
    return true;
  });
  const filtered_ids = filtered.map(paper => paper.id);
  const all_ids = papers.map(paper => paper.id);
  const hidden_ids = all_ids.filter(id => !filtered_ids.includes(id));
  hidden_ids.forEach(id => hide(document.getElementById(id)));
  filtered_ids.forEach(id => show(document.getElementById(id)));

  const authors = {};
  filtered.forEach(paper => {
    paper.authors.forEach(author => {
      if (!(author in authors)) {
        authors[author] = 0;
      }
      authors[author] += 1;
    });
  });

  const tags = {};
  filtered.forEach(paper => {
    paper.tags.forEach(tag => {
      if (!(tag in tags)) {
        tags[tag] = 0;
      }
      tags[tag] += 1;
    });
  });

  const types = {};
  filtered.forEach(paper => {
    if (!(paper.type in types)) {
      types[paper.type] = 0;
    }
    types[paper.type] += 1;
  });

  function updateEntries(field, entries) {
    document.querySelectorAll(`.filter-${field} .category-entry`).forEach(entry => {
      const text = entry.querySelector('.category-text').innerText;
      const count = entries[text];
      const selectedField = typeof selected[field] === 'string' ? [selected[field]] : selected[field] || [];
      if (selectedField.includes(text)) {
        entry.classList.add('selected-entry');
      } else {
        entry.classList.remove('selected-entry');
      }
      entry.querySelector('.counter').innerText = `(${count || 0})`;
      if (!count) {
        entry.classList.add('empty-entry');
      } else {
        entry.classList.remove('empty-entry');
      }
    });
  }

  updateEntries('authors', authors);
  updateEntries('tags', tags);
  updateEntries('type', types);

  const decades = new Set();
  filtered.forEach(paper => decades.add(paper.year.substring(0, 3)));

  document.querySelectorAll('.decade-container').forEach(decade => {
    const decadeId = decade.id.slice(0, -2);
    const elements = [decade, ...decade.querySelectorAll('h1,h2')];
    elements.forEach(decades.has(decadeId) ? show : hide);
  });
}

document.querySelectorAll('.filter-authors .category-entry').forEach(entry => {
  entry.addEventListener('click', (e) => {
    const author = entry.querySelector('.category-text').innerText;
    if (_selected.authors.includes(author)) {
      _selected.authors = _selected.authors.filter((a) => a !== author);
    } else {
      _selected.authors.push(author);
    }
    update(_papers, _selected);
  });
});

document.querySelectorAll('.filter-tags .category-entry').forEach(entry => {
  entry.addEventListener('click', (e) => {
    const tag = entry.querySelector('.category-text').innerText;
    if (_selected.tags.includes(tag)) {
      _selected.tags = _selected.tags.filter((a) => a !== tag);
    } else {
      _selected.tags.push(tag);
    }
    update(_papers, _selected);
  });
});

document.querySelectorAll('.filter-type .category-entry').forEach(entry => {
  entry.addEventListener('click', (e) => {
    const _type = entry.querySelector('.category-text').innerText;
    _selected.type = _selected.type === _type ? null : _type;
    update(_papers, _selected);
  });
});

update(_papers, _selected);
</script>
