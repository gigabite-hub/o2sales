/*
 *  Application specific jquery functions 
 */
(function($) {
  $.fn.equalHeights = function(minHeight, maxHeight) {
    tallest = (minHeight) ? minHeight : 0;
    this.each(function() {
      if($(this).height() > tallest) {
        tallest = $(this).height();
      }
    });
    var selector = this.selector;
    if((maxHeight) && tallest > maxHeight) tallest = maxHeight;
    return this.each(function() {
      $(this).height(tallest).css("overflow","hidden").addClass("setheight").data("heightsetby", selector);
    });
  }
  
  $.fn.simpleaccordion = function(opts) {
    var options = {
      "controlclosedtext": "More detail",
      "controlopentext": "Less detail"
    }
    
    return this.each(function() {
      var $content = $(this);
      $content.hide();
      $content.wrap('<div class="accordionelement"></div>')
      $content.before('<p class="accordionctrl" style="display:none;">'+options.controlclosedtext+'</p>');
      var $controller = $content.siblings('.accordionctrl').first();
      $controller.fadeIn();
      var $setheightparents = $content.parents('.setheight');
      $setheightparents.css('height','auto');
      $setheightparents.equalHeights();
      
      $controller.on('click', function(){
    	var $controller=$(this);
        if($content.is(':visible')){
          $content.slideUp(function(){
            $setheightparents.each(function(){
              var selector = $(this).data('heightsetby');
              $(selector).equalHeights();
            })
          });
          $controller.html(options.controlclosedtext)
          $controller.removeClass("open").addClass("closed");
          
        }else{
          $setheightparents.each(function(){
            
            $(this).css('height','auto');
          })
          $content.slideDown();
          $controller.html(options.controlopentext);
          $controller.removeClass("closed").addClass("open");
          
        }
      })
    });
  }
  
  $.fn.moveLabelsToPlaceholders = function(){
    return this.each(function(){
      //expects a form
      var $fieldsToProcess = $(this).find('input[type="text"], input[type="email"], input[type="password"]');
      $fieldsToProcess.each(function(){
          var $field = $(this);
          var id = $field.attr('id');
          var $label = $('label[for="'+id+'"]');
          $field.attr('placeholder', $label.text());
          $label.remove();
        })
    })
  }
  
  $.fn.showSignOut = function(){
	  return this.each(function(){
		  var $this = $(this);
		  var isloggedin = false;
		  var username;
		  if($.cookie('rAT') != null){
			isloggedin=true;
			username = $.cookie('username')
		  }
		  
		  if(isloggedin){
			//$this.find('.username').text(username);
		    $this.show();
		  }
	  })
  }
})(jQuery);


/*!
 * jQuery Cookie Plugin v1.3
 * https://github.com/carhartl/jquery-cookie
 *
 * Copyright 2011, Klaus Hartl
 * Dual licensed under the MIT or GPL Version 2 licenses.
 * http://www.opensource.org/licenses/mit-license.php
 * http://www.opensource.org/licenses/GPL-2.0
 */
(function ($, document, undefined) {

	var pluses = /\+/g;

	function raw(s) {
		return s;
	}

	function decoded(s) {
		return decodeURIComponent(s.replace(pluses, ' '));
	}

	var config = $.cookie = function (key, value, options) {

		// write
		if (value !== undefined) {
			options = $.extend({}, config.defaults, options);

			if (value === null) {
				options.expires = -1;
			}

			if (typeof options.expires === 'number') {
				var days = options.expires, t = options.expires = new Date();
				t.setDate(t.getDate() + days);
			}

			value = config.json ? JSON.stringify(value) : String(value);

			return (document.cookie = [
				encodeURIComponent(key), '=', config.raw ? value : encodeURIComponent(value),
				options.expires ? '; expires=' + options.expires.toUTCString() : '', // use expires attribute, max-age is not supported by IE
				options.path    ? '; path=' + options.path : '',
				options.domain  ? '; domain=' + options.domain : '',
				options.secure  ? '; secure' : ''
			].join(''));
		}

		// read
		var decode = config.raw ? raw : decoded;
		var cookies = document.cookie.split('; ');
		for (var i = 0, l = cookies.length; i < l; i++) {
			var parts = cookies[i].split('=');
			if (decode(parts.shift()) === key) {
				var cookie = decode(parts.join('='));
				return config.json ? JSON.parse(cookie) : cookie;
			}
		}

		return null;
	};

	config.defaults = {};

	$.removeCookie = function (key, options) {
		if ($.cookie(key) !== null) {
			$.cookie(key, null, options);
			return true;
		}
		return false;
	};

})(jQuery, document);
