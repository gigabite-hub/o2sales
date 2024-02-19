(function($) {
  $.fn.enableSubmit = function(enabled) {
    return this.each(function() {
      var $form = $(this);
      var $submitbtns = $form.find('[type="submit"]');
      var $submitbtn = $submitbtns.not('[name="cancel"]');
		  if (enabled) {
			  $submitbtn.prop('disabled', false);
			  $submitbtn.removeClass('disabled');
		  } else {
			  $submitbtn.prop('disabled', true);
			  $submitbtn.addClass('disabled');
		  }
    })
  }
})(jQuery);

(function($) {
  $.fn.fieldvalidator = function(opts) {
    var options = {
      "msgpattern" : "multiple"
    };

    return this.each(function() {
      if (opts) {
        $.extend(options, opts);
      }
      
      var $field = $(this);
      var $form = $field.closest('form');
      var $messagearea = $(options.validationmessagearea);
      var fieldvalisvalid = false;
           
     

      var regexmatched = function($field, regexp) {
        if($field.val().match(regexp)==null){
          return false;
        }else{
          return true;
        }        
      }
      var checkequality = function($field1, $field2) {
        if ($field1.val() === $field2.val() && $field1.val() !== "") {
          return true;
        } else {
          return false;
        }
      }
      var setfieldstate = function($field) {
        if (fieldvalisvalid) {
          $field.removeClass("invalid");
          $field.addClass("valid");
        } else {
          $field.removeClass("valid");
          $field.addClass("invalid");
        }
      }
      var setmsgvisibility = function() {
        var shownfirstmessage = false;
        $messagearea.children().hide();
        $messagearea.children().each(function() {
          var $this = $(this);
          if (shownfirstmessage) {
            return (false);
          }          
          if ($this.hasClass('validated')) {
            $this.hide();
          } else if ($this.hasClass('unvalidated')) {
            $this.show();
            //$messagearea.show();
            shownfirstmessage = true;
          } else {
            $this.show();
            //$messagearea.show();
            shownfirstmessage = true;
          }
        })
        if(!shownfirstmessage){
          var $successmessage = $messagearea.children().first().clone().appendTo($messagearea);
          $successmessage.html('<em> </em>'+options.successmessage).addClass("success").show()
        }else{
          $messagearea.children('.success').hide();
        }
      }
      var markvalidity = function(isvalid, $msg, forceValidate) {
        if (isvalid) {
          $msg.data("hasvalidated", true);
          $msg.removeClass("unvalidated");
          $msg.addClass("validated");
        } else {
          fieldvalisvalid = false;
          if ($msg.data("hasvalidated") || forceValidate) {
            $msg.data("hasvalidated", true);
            $msg.addClass("unvalidated");
          }
          $msg.removeClass("validated");
        }
        
      }
      var resetStatus = function($msg){
        $msg.data("hasvalidated", false);
        $msg.removeClass("unvalidated validated");
        $field.removeClass("valid").addClass("invalid")
      }
      
      var validate = function() {
        fieldvalisvalid = true;
        if(!$field.prop('disabled')){
        for (var pos = 0; pos < options.validationrules.length; pos++) {
          var validationrule = options.validationrules[pos];
          var testtype = validationrule.type;
          var $msg = $messagearea.find('.' + validationrule.identifier);
          var forceValidate = validationrule.alwaysValidateUnlessEmpty;

          if($field.val() == ""){
            resetStatus($msg)
          }else{
        	switch(testtype) {
              case "regex":
                markvalidity(regexmatched($field, validationrule.test), $msg, forceValidate);
                setfieldstate($field);
                break;
              case "birthdate":
              	var test = false;
              	var $datefields;
              	if ( typeof (validationrule.test) == "object") {
              		var dd = $(validationrule.test[0]).val();
              		var dm = $(validationrule.test[1]).val();
              		var dy = $(validationrule.test[2]).val();
              	    $datefields = $(validationrule.test[0]+", "+ validationrule.test[1]+", "+ validationrule.test[2])
              		var datestr = dd + "/" + dm + "/" + dy; 
              		var dateToValidate = Date.parseExact(datestr,"d/M/yyyy");
              		var currentDate = Date.today();
              		if(dateToValidate != null && !isNaN(dateToValidate)) {
              			
              			test = (currentDate > dateToValidate);
              			if (dd < 10) {
              				dd = '0' + dd;
              			}
              			if (dm < 10) {
              				dm = '0' + dm;
              			}
              			$(validationrule.test[3]).val(dd + "/" + dm + "/" + dy);
              		}
              		markvalidity(test, $msg, forceValidate);
              	}
              	setfieldstate($datefields);
                  break;
              case "length":
                var test = false;
                if ( typeof (validationrule.test) == "object") {
                  if (validationrule.test.length > 1) {
                    test = ($field.val().length >= validationrule.test[0]) && ($field.val().length <= validationrule.test[1]) && ($.trim($field.val()).length > 0);
                  } else {
                    test = ($field.val().length >= validationrule.test[0])  && ($.trim($field.val()).length > 0);
                  }
                } else if ( typeof (validationrule.test) == "number") {
                  test = ($field.val().length >= validationrule.test)  && ($.trim($field.val()).length > 0);
                }

                markvalidity(test, $msg, forceValidate);
                setfieldstate($field);
                break;
              case "matchvalue":
                var $testfield = $(validationrule.test);
                markvalidity(checkequality($field, $testfield), $msg, forceValidate);
                setfieldstate($field);
                $testfield.off();
                $testfield.on('keyup mouseup input change', function(e){
                  $field.triggerHandler('change');
                });
                break;
              default:
                break;
            }
          }
          
        }
        }
        
        if (options.msgpattern == "single") {
          setmsgvisibility();
        }
      }
      
      if($messagearea.length > 0){
	      var msgstr = "";
	      for (var pos = 0; pos < options.validationrules.length; pos++) {
	        var validationrule = options.validationrules[pos];
	        var messages = (validationrule.failureMsg)
	        	? '<p class="success">' + validationrule.msg + '</p><p class="failure">' + validationrule.failureMsg + '</p>'
	        	: '<p>' + validationrule.msg + '</p>';

	        msgstr += '<li data-rule="' + validationrule.type + pos + '" class="' + validationrule.type + pos + '"><em> </em>' + messages + '</li>';
	        validationrule.identifier = validationrule.type + pos
	      };
	      if (options.msgpattern == "multiple") {
	        $(options.validationmessagearea).html(msgstr);
	      } else {
	        var prefix = $messagearea.prev('.validationprefix').text();
	        var $pwlabel = $('label[for="' + $field.attr('id') + '"]');
	        $messagearea.addClass('showone');
	        $messagearea.parent().addClass('showone');
	        $messagearea.insertBefore($field);
	        $messagearea.html(msgstr);
	        $messagearea.children().each(function(){
	          var $message = $(this);
	          if(prefix != ""){
	            $message.html('<em> </em>'+prefix+' '+$message.text());
	          }
	          $message.hide();
	        })
	        $messagearea.children().first().show();
	      }
	      
	      $messagearea.show();
      }
      
      $field.on('keyup mouseup input change', validate);

      /* Fix for IE8 and IE9 */
      var placeholder = $field.attr('placeholder');
      if($field.val() != "" && $field.val() != placeholder) validate();
    });
  };
})(jQuery);

(function($) {
  $.fn.formvalidator = function(opts) {
    var options = {
      "liveupdate" : true
    }
    
    
    return this.each(function() {
      if (opts) {
        $.extend(options, opts);
      }

      var $form = $(this);
      var $buttonContainer = $(options.buttonContainerSelector || this);

      var $requiredfields = $form.find('[required="required"]');
      
      var setsubmitstate = function($testfields){
        var submitenabled = true;
        $testfields.each(function(){
          var $field = $(this);
          if(!$field.prop('disabled')){
	        if($field.hasClass('invalid') || $.trim($field.val())=="" || ($field.is('select') && ($field.val()==-1 || $field.val()=='select')) || ($field.is(':checkbox') && !$field.is(':checked'))){
	           submitenabled = false;
	         }
	       }
        });
        
        $buttonContainer.enableSubmit(submitenabled);
      };
      
      if(options.liveupdate){
        setsubmitstate($requiredfields);
      
        $requiredfields.on("keyup mouseup input change", function() {
          $('.collapseErrorsOnChange div.error').slideUp(200);
          setsubmitstate($requiredfields);
        });
      }
      
      $form.on('submit', function(){
        $buttonContainer.enableSubmit(false);
      });
      
    });
  }
})(jQuery);

function toggleSignInAutomaticMessage(className, obj) {
    $(className).toggle( !obj.checked )
}