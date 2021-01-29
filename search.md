---
layout: page
title: Search
---

<!-- HTML elements for search -->
<div style="text-align:center">
<span style="font-size:18px">search:</span><input type="text" id="search-input" style="
    width:30%;
    outline-style:none;
    border: 2px solid #b5e853;
    background-color: #1b1b1b;
    color: white;
    padding: 5px 5px;
"/>
</div>

<ul id="results-container"></ul>

<!-- script pointing to jekyll-search.js -->
<script src="/js/simple-jekyll-search.min.js"></script>

<script>
SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('results-container'),
    json: '/search.json',
    searchResultTemplate: '<li><a href="{url}" title="{desc}">{title}</a></li>',
    noResultsText: '没有搜索到文章',
    limit: 20,
    fuzzy: false
  })
</script>
