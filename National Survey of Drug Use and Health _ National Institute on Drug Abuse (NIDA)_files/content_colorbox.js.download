(function ($) {
  Drupal.behaviors.cboxLoadedContent = {
    attach: function (context, setting) {
      if (context == '#cboxLoadedContent') {
        var element = $.colorbox.element()[0];

        if (typeof element != 'undefined') {
          var $this = $(element);
          var contentHtml = $this.parent('div.border').html();

          if ($(contentHtml).length) {
            contentHtml = $('<div/>').append(contentHtml);

            //remove the colorbox link
            contentHtml.find('a.colorbox').remove();

            var trimmedHtml = $(contentHtml).html();
            if ($(trimmedHtml).length) {
              if (trimmedHtml.length >= 299) {
                trimmedHtml = $.trim(trimmedHtml).substring(0, 300).split(" ").slice(0, -1).join(" ") + "...";
                console.log(trimmedHtml);
                trimmedHtml = $('<div/>').append(trimmedHtml);
                $('#cboxLoadedContent').append(trimmedHtml);
              } else {
                
                $('#cboxLoadedContent').append(contentHtml);
              }
              

            }
            $.colorbox.resize();
          }
        }
      }
    }
  }
})(jQuery);