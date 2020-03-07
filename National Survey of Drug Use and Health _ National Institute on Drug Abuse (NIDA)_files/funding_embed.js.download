(function($) {
	Drupal.behaviors.funding_embed = {
		attach : function(context, settings) {
			var fs = $('#edit-funding-embed');

			if (fs.length) {
				var cb = fs.find('input.form-checkbox');
				var terms = fs.find('input.form-checkbox').filter(':not([name*="type"])').filter(':not([name*="status"])');
				var types = fs.find('input.form-checkbox[name*="type"]');
				var status = fs.find('input.form-checkbox[name*="status"]');
				var r = $('#edit-shortcode-result-textarea');
				var h = "[funding {terms} {types} {status}][/funding]";

				cb.change(function() {
					var ids = ts = ss = "";

					terms.filter(':checked').each(function() {
						ids += $(this).val() + ',';
					});
					ids = ids.substr(0, ids.length - 1);

					types.filter(':checked').each(function() {
						ts += $(this).val() + ',';
					});
					ts = ts.substr(0, ts.length - 1);

					status.filter(':checked').each(function() {
						ss += $(this).val() + ',';
					});
					ss = ss.substr(0, ss.length - 1);

					var o = h;

					o = o.replace('{terms}', ids.length ? 'terms=' + ids : '');
					o = o.replace('{types}', ts.length ? 'types=' + ts : '');
					o = o.replace('{status}', ss.length ? 'status=' + ss : '');

					r.val(o);
				});

				r.change(function() {
					var shortcode = $(this).val();

					var terms = fs.find('input.form-checkbox').filter(':not([name*="type"])').filter(':not([name*="status"])');
					var types = fs.find('input.form-checkbox[name*="type"]');
					var status = fs.find('input.form-checkbox[name*="status"]');

					var _terms = shortcode.match(/terms=(.+?)[\s\]]/);
					var _types = shortcode.match(/types=(.+?)[\s\]]/);
					var _status = shortcode.match(/status=(.+?)[\s\]]/);

					if (_terms != null && _terms.length) {
						$.each(_terms[1].split(','), function(i, e) {
							terms.filter('[value=' + e + ']').attr('checked', 'checked');
						});
					}

					if (_types != null && _types.length) {
						$.each(_types[1].split(','), function(i, e) {
							types.filter('[value=' + e + ']').attr('checked', 'checked');
						});
					}

					if (_status != null && _status.length) {
						$.each(_status[1].split(','), function(i, e) {
							status.filter('[value=' + e + ']').attr('checked', 'checked');
						});
					}
				});
			}
		}
	};
})(jQuery);