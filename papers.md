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
  font-size: 12px;
  color: white;
  background-color: #007bff;
  padding: 0.1rem 0.4rem;
  margin-left: 0.1rem;
  margin-right: 0.1rem;
  border-radius: 0.3rem;
}
</style>

**Todo:** Add Searching / Filtering / Tagging
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

{%
  assign papers_grouped = papers |
  group_by_exp: 'd', 'd.year  | truncate: 3, ""' |
  sort: 'name'
%}
{% for paper_grouped in papers_grouped %}
  <h1 class="decade-header">{{paper_grouped.name}}0s</h1>
  {% assign items = paper_grouped.items | sort: 'year' %}
  {% for paper in items %}
  <div id="{{paper.name}}">
    <h2 class="title">
      <span class="title-year">{{paper.year}}</span>
      {{paper.title}}
    </h2>
    {{paper.authors[0]}}
    {% for author in paper.authors offset:1 %}
      <span style='color: lightgray'>&#9679;</span> {{author}}
    {% endfor %}
    <div class="d-flex">
      {% for tag in paper.tags %}
      <div class="tag">{{ tag }}</div>
      {% endfor %}
    </div>
    {{paper.content}}
  </div>
  {% endfor %}
{% endfor %}
