/**
 * Array.filter() function
 * doesn't exist in IE 8, so we add it ourselves
 */
if (!Array.prototype.filter) {
  Array.prototype.filter = function (fun /*, thisp */) {
    "use strict";

    if (this === void 0 || this === null)
      throw new TypeError();

    var t = Object(this);
    var len = t.length >>> 0;
    if (typeof fun !== "function")
      throw new TypeError();

    var res = [];
    var thisp = arguments[1];
    for (var i = 0; i < len; i++) {
      if (i in t) {
        var val = t[i]; // in case fun mutates this
        if (fun.call(thisp, val, i, t))
          res.push(val);
      }
    }

    return res;
  };
}

(function ($) {
  Drupal.behaviors.pubfinder = {
    attach: function (context, setting) {
      var $shortSearchForm = $('#pubfinder-short-search-form', context);
      var $advnSearchForm = $('#pubfinder-search-form', context);
      var $advnSearchWrapper = $('#pubfinder-search-form fieldset.vocabularies-wrapper', context);
      var $refineSearchForm = $('#pubfinder-search-refine-form', context);
      var $refineSearchWrapper = $('#pubfinder-search-refine-form fieldset.vocabularies-wrapper', context);
      var $advnSearchToggle = $('<a>', {'class': 'advanced-search-toggle'}).text('Advanced Search');

      // on android or iphone
      if (/iP(od|hone)/i.test(window.navigator.userAgent) ||
        /Android/i.test(window.navigator.userAgent)) {
        $(document).unbind('chosen:showing_dropdown')
          .on('chosen:showing_dropdown', function (evt, param) {
            if (typeof param.chosen.container != 'undefined') {
              $('html, body').animate({
                scrollTop: $(param.chosen.container).offset().top - 30
              }, 700);
            }
          });
      }

      // refine search form
      if ($refineSearchWrapper.length) {
        $refineSearchWrapper.find('select').chosen();
      }

      // short search form
      if ($shortSearchForm.length) {
        $shortSearchForm.find('select').chosen();
      }

      // full search form
      if ($advnSearchWrapper.length) {
        $advnSearchWrapper.find('select').chosen();

        /**
         * adds the advanced search toggle button
         */
        $advnSearchWrapper.before($advnSearchToggle);

        /**
         * handles the advanced search toggle button click
         * to open and close the checkbox wrapper
         */
        $advnSearchToggle.click(function () {
          $('.advanced-search-toggle').toggleClass('active');

          if ($advnSearchWrapper.hasClass('open')) {
            $advnSearchWrapper.hide().removeClass('open');
            $advnSearchWrapper
              .find('input[type="select"] option')
              .prop('selected', false);
            $advnSearchForm.find('input.main-submit').show();
          } else {
            $advnSearchWrapper.show().addClass('open');
            $advnSearchForm.find('input.main-submit').hide();
          }
          return false;
        });

        /**
         * do not hide checkboxes if any of them is checked
         */
        if ($advnSearchWrapper.find('.form-type-select.selected').length) {
          $advnSearchWrapper.show();
        } else {
          $advnSearchWrapper.hide();
        }

        $advnSearchWrapper.addClass('advn-search-processed');
      }
    }
  };
})
(jQuery);