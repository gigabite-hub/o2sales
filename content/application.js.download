/* Browser and JS detection and messaging */
//if js is enabled and browser is old
jQuery(function($){
  $('#javascriptWarning').remove();
  if ($.browser.msie && $.browser.version < 7) {
    $('#browserWarning').show();
  } else {
    $('#browserWarning').remove();
  }
  if(!Modernizr.cookies){
	  $('#generalWarnings').append('<div id="cookieWarning">Your browser does not have Cookies enabled and therefore some features of this and other websites may not function.</div>')
  }
})