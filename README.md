```javascript
;(function($){
	$(function()
	{
		var container = BODY.find('main.container');
		var content = container.find('#content');
		var sidebar = container.find('aside');
		var info_content = content.find('.info_content');
		var article_md_view = content.find('article.md_view');
		var popover_original_nolink = content.find('#popover_original_nolink');
		var copyright_bar = info_content.find('#copyright_bar');
		var copyright_bar_svg = copyright_bar.find('svg');
		var copyright_bar_type = '';

		copyright_bar_svg.each
		(
			function(inx, elem)
			{
				_this = $(elem);

				copyright_bar_type = _this.data("type");

				_this.popover
				({
					'container':'body',
					'html':true,
					'title':copyright.text[copyright_bar_type].title,
					'content':$.utils.isObject(copyright.text[copyright_bar_type].info, 'function') ? copyright.text[copyright_bar_type].info(ARTICLE_UID, ARTICLE_USERNAME) : copyright.text[copyright_bar_type].info,
					'template':'<div class="popover shadow popover-copyright-' + copyright_bar_type + '" role="tooltip"><h3 class="popover-title font12"></h3><div class="popover-content text-muted font12"></div></div>',
					'placement':'top',
					'delay':{'show':200, 'hide':200},
					'trigger':'manual'
				});

				if(copyright_bar_type == 'business')
				{
					popover_hover_events
					(
						_this, 'popover-copyright-' + copyright_bar_type, function(_this, elem)
						{
							send_message(null, elem);
						}
					);
				}
				else
				{
					popover_hover_events(_this, 'popover-copyright-' + copyright_bar_type);
				}
			}
		);

		var popover_nolink = "<span>因转载多次，原地址不详！您若知道原地址，请联系</span><a href='javascript:;' class='primary hand' data-toggle='sendmsg' data-event='false' data-touid='" + ARTICLE_UID + "'>" + ARTICLE_USERNAME + "</a><span>，谢谢。</span>";
		if(popover_original_nolink.length > 0)
		{
			popover_original_nolink.popover
			({
				'container':'body',
				'html':true,
				'title':'为尊重作者，特此声明：',
				'content':popover_nolink,
				'template':'<div class="popover shadow popover-original-nolink" role="tooltip"><h3 class="popover-title font12"></h3><div class="popover-content text-muted font12"></div></div>',
				'placement':'top',
				'delay':{'show':200, 'hide':200},
				'trigger':'manual'
			});

			popover_hover_events(popover_original_nolink, 'popover-original-nolink', function(_this, elem){send_message(null, elem);});
		}

		flowcharts.refresh(article_md_view);
		flowcharts.zoom(article_md_view);
		sequence.refresh(article_md_view);
		sequence.zoom(article_md_view);
		// article_md_view.find('pre[data-codetype]').each
		BODY.find('pre[data-codetype]').each
		(
			function(inx, elem)
			{
				elem = $(elem);
				codebox.precode.create(elem, elem.data("codetype"));
			}
		);
	});
})(jQuery);
```
