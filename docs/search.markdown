---
layout: page
title: Search
permalink: /search/
---

<!-- Html Elements for Search -->
<div id="search-container">
<center><h1><b>Search</b></h1></center>

<input type="text" id="search-input" placeholder="Search Articles" style="width: 100%; padding: 12px 20px; margin: 8px 0; box-sizing: border-box; border: 5px solid #5575FC; border-radius: 25px;">
<ul id="results-container"></ul>

<!-- Script pointing to search-script.js -->
<script src="/assets/js/simple-jekyll-search.min.js" type="text/javascript"></script>

<!-- Configuration -->
<script>
SimpleJekyllSearch({
  searchInput: document.getElementById('search-input'),
  resultsContainer: document.getElementById('results-container'),
  json: '/search.json',
  searchResultTemplate: '<div><a href="{url}"><h1>{title}</h1></a><span>{date}</span></div>'
})
</script>
