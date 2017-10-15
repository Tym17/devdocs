{% include header.html %}

<div class="search-results-header">
  <div id="search-input"></div>
</div>

<div class="search-results container">

  <div id="stats"></div>
  <div id="hits"></div>
  <div id="pagination"></div>

</div><!-- container -->

<link rel="stylesheet" href="https://cdn.jsdelivr.net/instantsearch.js/1/instantsearch.min.css">
<script src="https://cdn.jsdelivr.net/instantsearch.js/1/instantsearch.min.js"></script>

<script>

  app({
    appId: "{{ site.algolia.application_id }}",
    apiKey: "{{ site.algolia.api_key }}",
    indexName: "{{ site.algolia.index_name }}",
    urlSync: true
  });



  function app(opts) {
    var switcher = document.getElementsByClassName('version-switcher')[0];
    var ver = String( switcher.dataset.version );
    const search = instantsearch({
      appId: opts.appId,
      apiKey: opts.apiKey,
      indexName: opts.indexName,
      urlSync: true,
      searchParameters: {

        facetsRefinements: {
          guide_version: [ ver ],
        },
        facets: ['guide_version'],
      }
    });

    search.addWidget(
      instantsearch.widgets.searchBox({
        container: '#search-input',
        placeholder: 'Search',
      })
    );

    search.addWidget(
      instantsearch.widgets.hits({
        container: '#hits',
        hitsPerPage: 10,
        templates: {
          item: function ( item ) {
            var title_highlighted = item._highlightResult.title;
            var title_plain = item.title;
            var url = item.url;
            // Check if we can show title, if not - at least show URL
            if (typeof title_highlighted !== 'undefined') {
              title = title_highlighted.value;
            } else if (typeof title_plain !== 'undefined') {
              title = title_plain;
            } else {
              title = url;
            }
            var link = '<a href="' + url + '">' + title +'</a>';

            return '<div class="hit"><h2 class="hit-name">'+ link + '</h2><div class="hit-content">'+ item._highlightResult.text.value + '</div></div>';
          },
          empty: function ( query ) {
            return '<div id="no-results-message"><p>No results for the search <em>"' + query.query +'"</em>.</p></div>';
          },
        },
      })
    );

    search.addWidget(
      instantsearch.widgets.stats({
        container: '#stats',
      })
    );




    search.addWidget(
      instantsearch.widgets.pagination({
        container: '#pagination',
        scrollTo: '#search-input',
      })
    );

    search.start();
  }

</script>



{% include footer.html %}
