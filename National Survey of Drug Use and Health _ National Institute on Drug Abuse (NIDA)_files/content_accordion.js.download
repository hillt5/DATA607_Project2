(function ($) {
  Drupal.behaviors.content_accordion = {
    attach: function (context, setting) {
      var $toggleButton = $('#content_accordion_toggle', context);
      var toggleOpen = true;

      if ($toggleButton.length) {
        $toggleButton.click(function () {
          $('.content-accordion', context).each(function () {
            var $wrapper = $(this);
            if (toggleOpen) {
              toggleAccordion($wrapper, 'open');
            }
            else {
              toggleAccordion($wrapper, 'close');
            }
          });

          if (toggleOpen) {
            $toggleButton.text(Drupal.t('Collapse All'));
            toggleOpen = false;
          } else {
            $toggleButton.text(Drupal.t('Expand All'));
            toggleOpen = true;
          }
        });
      }

      //body-based views accordion. for example only.
      $('.content-accordion', context).each(function () {
        var $wrapper = $(this),
          $header = $wrapper.children('.accordion-header'),
          $content = $wrapper.children('.accordion-content');

        //$content.hide();
        $content.css("height", "0");
        $header.click(function () {
          if ($wrapper.hasClass('open')) {
            toggleAccordion($wrapper, 'close');
          } else {
            toggleAccordion($wrapper, 'open');
          }
        });
      });

      if ("onhashchange" in window) {
        $(window).bind('hashchange', function () {
          var newHash = window.location.hash;

          goToHash(newHash);
        });
      }

      goToHash(window.location.hash);

      function goToHash(hash) {
        if (hash) {
          // go to hash by header
          var $header = $('h5' + hash, context);

          if ($header.length) {
            if ($header.parent('div').hasClass('accordion-header')) {
              $header.click();
              $("html, body").animate({scrollTop: $header.offset().top - 50}, 500);
            }
          } else {
            // go to hash by link in content across accordions
            var $hash = $(hash, context),
              $content = $hash.parents('.accordion-content');

            if ($hash.length && $content.length) {
              $header = $content.prev('.accordion-header').children('h5');

              if ($header) {
                $header.click();
                setTimeout(function () {
                  $("html, body").animate({scrollTop: $hash.offset().top - 50}, 500);
                }, 250);
              }
            }
          }
        }
      }

      function toggleAccordion($accordion, state) {
        if (state === 'open') {
          if (!$accordion.hasClass('open')) {
            $accordion.addClass('open');
            $accordion.children('.accordion-content').transition({height: 'auto'});
          }
        } else {
          if ($accordion.hasClass('open')) {
            $accordion.removeClass('open');
            $accordion.children('.accordion-content').transition({height: '0'});
          }
        }
      }
    }
  };
})(jQuery);
