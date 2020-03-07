(function ($, undefined) {
  Drupal.behaviors.pubfinder_top_nodes = {
    attach: function (context, settings) {
      var wrappers = $('div.pubfinder-top-nodes div.pubfinder-lazy-content', context);
      var CSS_CONTENT_LOADING = 'pubfinder-content-loading';
      var CSS_CONTENT_PROCESSED = 'pubfinder-content-processed';
      var CSS_NO_CONTENT = 'pubfinder-no-content';
      var query = '';
      var taxonomy = [];
      
      if (settings.pubfinder !== undefined) {
        if (settings.pubfinder.query !== undefined) {
          query = settings.pubfinder.query;
        }

        if (settings.pubfinder.taxonomy !== undefined) {
          taxonomy = settings.pubfinder.taxonomy;
        }

        wrappers.each(function () {
          var $this = $(this);
          var content_type = $this.attr('data-content-type');
          var delta = $this.attr('data-delta');

          if (content_type) {
            $this.addClass(CSS_CONTENT_LOADING);

            $.ajax(settings.pubfinder.base_url + '/ajax/top_nodes', {
              type: 'post',
              data: {
                'type': content_type,
                'query': query,
                'taxonomy': taxonomy,
                'delta': delta
              },
              dataType: 'json',
              success: function (result) {
                if (result && result.count > 0) {
                  $this.children('div.content').html(result.data);
                } else {
                  $this.addClass(CSS_NO_CONTENT);
                }
              },
              complete: function () {
                $this
                  .removeClass(CSS_CONTENT_LOADING)
                  .addClass(CSS_CONTENT_PROCESSED);
              }
            });
          }
        });
      }
    }
  };
})(jQuery);