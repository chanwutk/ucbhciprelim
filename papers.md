---
title: Papers
layout: default
---
<style>
.title {
  padding: 0px;
  margin: 0px;
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
  margin-right: 0.4rem;
  border-radius: 0.2rem;
}

.paper-detail {
  margin-left: 5px;
}

.decade-papers {
  margin-left: 10px;
}

.counter {
  color: grey;
}

.category-container {
  height: 300px;
  overflow-y: auto;
}

.filter-pannel {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.category-entry {
  margin-right: 5px;
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

**Todo:** Add Filtering Interaction
{% assign papers = site.papers %}

<script>
(function() {
  const a = [
    {% for paper in papers %}
      {
        title: '{{ paper.title }}',
        year: '{{ paper.year }}',
        authors: '{{ paper.authors }}',
        id: '{{ paper.name }}',
        type: '{{ paper.type }}',
        tags: '{{ paper.tags }}',
        content: '{{ paper.content }}',
      },
    {% endfor %}
  ];
  console.log(a);
})();
</script>

<div class="filter-pannel">
  <div class="filter-selector">
    {% assign all_tags = "" | split: ',' %}
    {% for paper in papers %}
      {% for tag in paper.authors %}
        {% unless all_tags contains tag %}
          {% assign all_tags = all_tags | push: tag %}
        {% endunless %}
      {% endfor %}
    {% endfor %}
    {% assign all_tags_with_count = "" | split: ',' %}
    {% for tag in all_tags %}
      {% assign paper_count = 0 %}
      {% for paper in papers %}
        {% if paper.authors contains tag %}
          {% assign paper_count = paper_count | plus: 1 %}
        {% endif %}
      {% endfor %}
      {% assign paper_count_0 = paper_count | modulo: 10 %}
      {% assign paper_count_1 = paper_count | divided_by: 10 | modulo: 10 %}
      {% assign paper_count_2 = paper_count | divided_by: 100 | modulo: 10 %}
      {% assign paper_count_str = paper_count_2 | append: paper_count_1 | append: paper_count_0 %}
      {% assign t = tag | split: '___' | unshift: paper_count | unshift: paper_count_str | join: "___" %}
      {% assign all_tags_with_count = all_tags_with_count | push: t %}
    {% endfor %}
    {% assign all_tags_with_count = all_tags_with_count | sort | reverse %}
    <h3>Author <span class="counter">({{all_tags_with_count | size}})</span></h3>
    <div class="category-container">
      {% for t in all_tags_with_count %}
        {% assign t2 = t | split: '___' %}
        <div class="category-entry">
          {{t2[2]}} <span class="counter">({{t2[1]}})</span>
        </div>
      {% endfor %}
    </div>
  </div>
  <div class="filter-selector">
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
      {% assign t = tag | split: '___' | unshift: paper_count | unshift: paper_count_str | join: "___" %}
      {% assign all_tags_with_count = all_tags_with_count | push: t %}
    {% endfor %}
    {% assign all_tags_with_count = all_tags_with_count | sort | reverse %}
    <h3>Tag <span class="counter">({{all_tags_with_count | size}})</span></h3>
    <div class="category-container">
      {% for t in all_tags_with_count %}
        {% assign t2 = t | split: '___' %}
        <div class="category-entry">
          {{t2[2]}} <span class="counter">({{t2[1]}})</span>
        </div>
      {% endfor %}
    </div>
  </div>
  <div class="filter-selector">
    {%
      assign papers_type_grouped = papers |
      group_by: 'type' |
      sort: 'size' |
      reverse
    %}
    <h3>Type <span class="counter">({{papers_type_grouped | size}})</span></h3>
    <div class="category-container">
      {% for type in papers_type_grouped %}
        <div class="category-entry">
          {{ type.name }} <span class="counter">({{type.size}})</span>
        </div>
      {% endfor %}
    </div>
  </div>
</div>

{%
  assign papers_grouped = papers |
  group_by_exp: 'd', 'd.year  | truncate: 3, ""' |
  sort: 'name'
%}
{% for paper_grouped in papers_grouped %}
  <div class="d-flex">
    <h2 class="decade-left-header">{{paper_grouped.name}}0s</h2>
    <div class="decade-papers">
      <h1 class="decade-top-header">{{paper_grouped.name}}0s</h1>
      {% assign items = paper_grouped.items | sort: 'year' %}
      {% for paper in items %}
      <div id="{{paper.name}}">
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
          {{paper.content}}
        </div>
      </div>
      {% endfor %}
    </div>
  </div>
{% endfor %}
