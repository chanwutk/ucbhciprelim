---
title: Papers
layout: default
nav_order: 1
last_modified_date: August 29, 2023
---
<style>
.title {
  margin-bottom: 0px !important;
}

.title-year {
  color: grey;
  font-weight: normal;
}
.title-edit {
  font-size: .65em;
  opacity: 0.3;
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
          <a href="https://github.com/chanwutk/ucbhciprelim/edit/main/_papers/{{paper.name}}">
            <svg class='title-edit' xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 576 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M402.3 344.9l32-32c5-5 13.7-1.5 13.7 5.7V464c0 26.5-21.5 48-48 48H48c-26.5 0-48-21.5-48-48V112c0-26.5 21.5-48 48-48h273.5c7.1 0 10.7 8.6 5.7 13.7l-32 32c-1.5 1.5-3.5 2.3-5.7 2.3H48v352h352V350.5c0-2.1.8-4.1 2.3-5.6zm156.6-201.8L296.3 405.7l-90.4 10c-26.2 2.9-48.5-19.2-45.6-45.6l10-90.4L432.9 17.1c22.9-22.9 59.9-22.9 82.7 0l43.2 43.2c22.9 22.9 22.9 60 .1 82.8zM460.1 174L402 115.9 216.2 301.8l-7.3 65.3 65.3-7.3L460.1 174zm64.8-79.7l-43.2-43.2c-4.1-4.1-10.8-4.1-14.8 0L436 82l58.1 58.1 30.9-30.9c4-4.2 4-10.8-.1-14.9z"/></svg>
          </a>
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

Add new paper [Here](https://github.com/chanwutk/ucbhciprelim/new/main/?filename=_papers/year-paper.md&value=---%0Atitle: <paper name>%0Alayout: default%0Ayear: <year>%0Aauthors: [ <author1>, <author2>, ... ]%0Atags: [ <tag1>, <tag2>, ... ]%0Acitation: <citation>%0Atype: <type>%0Alinks: [ <link to paper pdf or doi>, ... ]%0Alink_descriptions: [ <link description>, ... ]%0A---)

Tags are modified from [Samantha Robertson](https://www.samantha-robertson.com/)'s notes.\
The paper filtering tool is inspired by [Dominik Moritz](https://www.domoritz.de)'s [publications](https://www.domoritz.de/publications/) page.

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
