(function($) {
	Drupal.behaviors.funding_widget = {
		attach : function(context, settings) {
			$('#fundingWidgetType').parent('div').addClass('widgetSelectButton');

			$('#fundingWidgetMobileForm').submit(function() {
				var feedURI = '/funding-app/feedapi?method=json&limit=200';
				var q = $.trim($('#fundingWidgetKeyword').val());
				var t = $.trim($('#fundingWidgetType').val());

				if (q.length) {
					feedURI += '&type=' + t + '&keyword=' + q;
					$.getJSON(feedURI, function(data) {
						$('#fundingWidgetResultList li').remove();
						if (data.length) {
							$("#fundingWidgetResultCount").text(data.length);
							$("#fundingWidgetResultKeyword").text(q);
							$("#fundingWidgetResultType").text($("#fundingWidgetType option:selected").text());

							$.each(data, function(key, val) {
								var h4 = '<h4>' + val.title + '</h4>';
								var content = '<p>' + val.slugline + ' ' + val.titleappend;

								if (t == 'rfa') {
									content = content + '<br />Letter of Intent Date: ' + val.lettersdate;
									content = content + '<br />Application Due Date: ' + val.appduedate;
								}
								content = content + '</p>';
								var link = '<a id="' + val.id + '" href="#page-result">' + h4 + content + '</a>';
								$('#fundingWidgetResultList').append('<li id="' + val.id + '" data-theme="d">' + link + '</li>');
								$('#fundingWidgetResultContainer').show();
							});
						}
					});
				}

				return false;
			});

			$('#fundingWidgetResultList li a').live('click', function() {
				var id = $.trim($(this).attr('id'));

				if (id) {
					var singleURI = '/funding-app/feedapi?method=json&id=' + id;

					$.getJSON(singleURI, function(data) {
						if (data.length) {
							$('#fundingWidgetResultContainer').hide();
							$('#fundingWidgetResultSingle h3').text(decodeURIComponent(data[0].title));
							$('#fundingWidgetResultSingle .singleContent').html(decodeURIComponent(data[0].contents));
							$('#fundingWidgetResultSingle').show();
						}
					});
				}

				return false;
			});

			$('#fundingWidgetResultSingle > a.back').click(function() {
				$('#fundingWidgetResultContainer').show();
				$('#fundingWidgetResultSingle').hide();
				return false;
			});
		}
	}
})(jQuery);