// $Id: extlink.js,v 1.8 2010/05/26 01:25:56 quicksketch Exp $
(function($) {
	function govextlinkAttach(context) {
		// Strip the host name down, removing ports, subdomains, or www.
		var pattern = /^(([^\/:]+?\.)*)([^\.:]{4,})((\.[a-z]{1,4})*)(:[0-9]{1,5})?$/;
		var host = window.location.host.replace(pattern, '$3$4');
		var subdomain = window.location.host.replace(pattern, '$1');

		// Determine what subdomains are considered internal.
		if (Drupal.settings.govextlink.extSubdomains) {
			var subdomains = "([^/]*\\.)?";
		} else if (subdomain == 'www.' || subdomain == '') {
			var subdomains = "(www\\.)?";
		} else {
			var subdomains = subdomain.replace(".", "\\.");
		}

		// Build regular expressions that define an internal link.
		var internal_link = new RegExp("^https?://" + subdomains + host, "i");

		// Extra internal link matching.
		var extInclude = false;
		if (Drupal.settings.govextlink.extInclude) {
			extInclude = new RegExp(Drupal.settings.govextlink.extInclude.replace(/\\/, '\\'));
		}

		// Extra external link matching.
		var extExclude = false;
		if (Drupal.settings.govextlink.extExclude) {
			extExclude = new RegExp(Drupal.settings.govextlink.extExclude.replace(/\\/, '\\'));
		}

    var cloudfront_link = new RegExp("cloudfront.net","i");

		// Find all links which are NOT internal and begin with http (as opposed
		// to ftp://, javascript:, etc. other kinds of links.
		// When operating on the 'this' variable, the host has been appended to
		// all links by the browser, even local ones.
		// In jQuery 1.1 and higher, we'd use a filter method here, but it is
		// not
		// available in jQuery 1.0 (Drupal 5 default).
		var external_links = new Array();
		var mailto_links = new Array();
		$("a:not(." + Drupal.settings.govextlink.extClass + ", ." + Drupal.settings.govextlink.mailtoClass + ")", context).each(
				function(el) {
					try {
						var url = this.href.toLowerCase();
            // it's an external link if it...
						if (url.indexOf('http') == 0 && // begins with http
							(!url.match(internal_link) || (extInclude && url.match(extInclude))) && // doesn't match custom rule
							!(extExclude && url.match(extExclude)) &&
              url.indexOf('.gov') === -1 &&  // is not .gov link
              !url.match(cloudfront_link) // is not a cloudfront link
            ){
								external_links.push(this);
						} else if (url.indexOf('mailto:') == 0) {
							mailto_links.push(this);
						}
					}
					// IE7 throws errors often when dealing with irregular
					// links, such as:
					// <a href="node/10"></a> Empty tags.
					// <a href="http://user:pass@example.com">example</a>
					// User:pass syntax.
					catch (error) {
						return false;
					}
				});

		if (Drupal.settings.govextlink.extClass) {
			// Apply the "ext" class to all links not containing images.
			if (parseFloat($().jquery) < 1.2) {
				$(external_links).not('[img]').addClass(Drupal.settings.govextlink.extClass).each(function() {
					if ($(this).css('display') == 'inline')
						$(this).after('<span class=' + Drupal.settings.govextlink.extClass + '></span>');
				});
			} else {
				$(external_links).not($(external_links).find('img').parents('a')).addClass(Drupal.settings.govextlink.extClass).each(function() {
					if ($(this).css('display') == 'inline')
						$(this).after('<span class=' + Drupal.settings.govextlink.extClass + '></span>');
						$(this).wrap('<span class="externalLink"/>');
						if($('html').attr('lang')=='es')
							$(this).parent('.externalLink').append('<span class="externalTooltip">Enlace externo, por favor revise nuestro <a href="/web-site-disclaimer">descargo de responsabilidad</a> (en ingl&eacute;s).</span>');
						else
							$(this).parent('.externalLink').append('<span class="externalTooltip">External link, please review our <a href="/web-site-disclaimer">disclaimer</a>.</span>');
				});
			}
		}

		if (Drupal.settings.govextlink.mailtoClass) {
			// Apply the "mailto" class to all mailto links not containing
			// images.
			if (parseFloat($().jquery) < 1.2) {
				$(mailto_links).not('[img]').addClass(Drupal.settings.govextlink.mailtoClass).each(function() {
					if ($(this).css('display') == 'inline')
						$(this).after('<span class=' + Drupal.settings.govextlink.mailtoClass + '></span>');
				});
			} else {
				$(mailto_links).not($(mailto_links).find('img').parents('a')).addClass(Drupal.settings.govextlink.mailtoClass).each(function() {
					if ($(this).css('display') == 'inline')
						$(this).after('<span class=' + Drupal.settings.govextlink.mailtoClass + '></span>');
				});
			}
		}

		if (Drupal.settings.govextlink.extTarget) {
			// Apply the target attribute to all links.
			$(external_links).attr('target', Drupal.settings.govextlink.extTarget);
		}

		if (Drupal.settings.govextlink.extAlert) {
			// Add pop-up click-through dialog.
			$(external_links).click(function(e) {
				return confirm(Drupal.settings.govextlink.extAlertText);
			});
		}

		// Work around for Internet Explorer box model problems.
		if (($.support && !($.support.boxModel === undefined) && !$.support.boxModel) || ($.browser.msie && parseInt($.browser.version) <= 7)) {
			$('span.ext, span.mailto').css('display', 'inline-block');
		}
	}

	Drupal.behaviors.govextlink = {
		attach : function(context) {
			govextlinkAttach(context);
		}
	};

})(jQuery);