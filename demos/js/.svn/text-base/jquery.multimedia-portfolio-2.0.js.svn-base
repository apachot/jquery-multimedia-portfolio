/* =========================================================
// jquery.multimedia-portfolio.js
// Author: OpenStudio (Arnault PACHOT)
// Mail: apachot@openstudio.fr
// Web: http://www.openstudio.fr
// Copyright (c) 2007-2010 OpenStudio
// licence : GPL
========================================================= */
(function($) {

$.fn.multimedia_portfolio = function(options) {
	this.each(function(){ 	
		var uniqueID = new Date();
		var rel_id=uniqueID.getTime();

		var mousewheelposition = 0;
		var defaultwidth = 320, defaultheight = 210;
		$(this).wrap("<div class='portfolio-container'></div>");
		var portfolio = $(this);
		var settings = { width: 700, baseDir: '.', nbelem: 3
		};
		if(options) $.extend(settings, options);
		
		var def_element_width = parseInt(settings.width/settings.nbelem);
		var portfolio_height = parseInt(settings.width/(settings.nbelem+1) + 50);
		
		var elements = $(this).children().not('.portfolio-loading-bar');
		var borderwidth = parseInt(((settings.width)/900)*7);
		var titlesize = (def_element_width/366);
		$('.portfolio-container').prepend("<div class='portfolio-bg-left'>&nbsp;</div><div class='portfolio-bg-right'>&nbsp;</div>").append("<div class='masque-left'>&nbsp;</div><div class='masque-right'>&nbsp;</div>");
		if (elements.length > settings.nbelem) $('.portfolio-container').append("<div class='portfolio-bg-bottom-left'>&nbsp;</div><div class='portfolio-bg-bottom-right'>&nbsp;</div>");
		var ratio_largeur = ((elements.length*def_element_width - settings.width) / (elements.length*def_element_width));
		$(".portfolio-container").css("width", settings.width+'px');
		if (elements.length > settings.nbelem) $(".portfolio-container").css("height", portfolio_height+'px'); else $(".portfolio-container").css("height", parseInt(portfolio_height-30)+'px');
		for ( var i = 0; i < elements.length; i++ ) {
				$(elements[i]).css('width', def_element_width+'px');
				$(elements[i]).find('img').not('.portfolio-mp3, .portfolio-loading-bar').each(function(){
				    $(this).addClass('img-type');
				});
				var currenthref, elementclass;
				if ((currenthref= $(elements[i]).children().filter("a").attr('href'))!= null) {
					var currentwidth = $(elements[i]).find('img').attr('width'); if (currentwidth==null) currentwidth=defaultwidth;
					var currentheight = $(elements[i]).find('img').attr('height'); if (currentheight==null) currentheight=defaultheight;
					var ratio = currentheight/((portfolio_height-35)*.7);///.6;
					currentwidth = parseInt(currentwidth/ratio);
					if (currentwidth > def_element_width-(borderwidth*2+6)) currentwidth = def_element_width-(borderwidth*2+6);
					currentheight = parseInt(currentheight/ratio);
					var currentstartimage = $(elements[i]).find('img').attr('src'); if (currentstartimage==null) currentstartimage='';
					var currenttitle = $(elements[i]).find('a').attr('title'); if (currenttitle==null) currenttitle='';
						
					
					if (currenthref.toLowerCase().indexOf('.pdf') > 0) {
						$(elements[i]).html("<iframe title='"+currenttitle+"' src='http://docs.google.com/gview?url="+currenthref+"&embedded=true' style='width:"+currentwidth+"px; height:"+currentheight+"px;' frameborder='0' />");
						elementclass = 'portfolio-pdf';
						
					} else if (currenthref.toLowerCase().indexOf('.flv') > 0) {
						$(elements[i]).empty().flash({swf: settings.baseDir+"/player_flv_maxi.swf", flashvars: {flv: currenthref, autoload: '0', showfullscreen: '1', bgcolor1: 'black', bgcolor2: 'black', startimage: currentstartimage, width: currentwidth, height: currentheight }, scale: 'showall', width: currentwidth, height: currentheight, allowfullscreen: 'true'});
						$(elements[i]).find('object').addClass('flv-type').attr('title', currenttitle);
						elementclass = 'portfolio-flv';
						
					} else if (currenthref.toLowerCase().indexOf('youtube') > 0) {
						var videoref=currenthref.substr(currenthref.lastIndexOf('v=')+2);
						$(elements[i]).empty().flash({swf: 'http://www.youtube.com/v/'+videoref+'&amp;fs=1&amp;rel=0', width: currentwidth, height: currentheight, allowfullscreen: true});
						$(elements[i]).find('object').addClass('flv-type').attr('title', currenttitle);
						elementclass = 'portfolio-youtube';
						
				      
					} else if (currenthref.toLowerCase().indexOf('.mp3') > 0) {
						$(elements[i]).empty().flash({swf: settings.baseDir+"/player_mp3_maxi.swf", flashvars: {mp3: currenthref, showslider: '1', width: (currentwidth-10), height: 20, bgcolor1: 'f2f2f2', bgcolor2: 'e6e6e6', buttoncolor: '000000', buttonovercolor: '00ad00'}, wmode: 'transparent', width: (currentwidth-10), height: 20});
						$(elements[i]).prepend("<img class='img-type' src='"+currentstartimage+"' width='"+currentwidth+"' height='"+currentheight+"' /></span>").find('object').addClass('mp3-type').attr('title', currenttitle).wrap("<span class='portfolio-mp3-container' style='top: "+(currentheight-20)+"px; margin-top: 0; margin-left: -"+parseInt((currentwidth-10)/2)+"px;'></span>");
						elementclass = 'portfolio-mp3';
					} else {
						if ($(elements[i]).find('img').length == 0){
						      $(elements[i]).find('a').html("<img class='img-type' src='"+currenthref+"' width='"+currentwidth+"' height='"+currentheight+"' alt='' />");
						}
						elementclass = 'portfolio-img';
					}
					$(elements[i]).find('.img-type, .flv-type, iframe').attr("width", currentwidth).attr("height", currentheight).wrap("<div class='portfolio-object-border' style='border-width: "+borderwidth+"px; width:"+currentwidth+"px;'></div>");
				}			
				var currenttitle;
				if ((currenttitle = $(elements[i]).find('a, object, iframe').attr("title")) != null) {
					$(elements[i]).addClass(elementclass).append("<div class='portfolio-title' style='font-size: "+titlesize+"em;'>"+currenttitle+"</div>");
				}
		};
		
		if (elements.length > settings.nbelem) {
		      $(".portfolio-container").append("<div class='slider-container' style='left: 66px; width:"+parseInt(settings.width-137)+"px'></div>");
		      $(".slider-container").append("<div class='ui-slider-1'></div>");
		      $(".ui-slider-1").css('width', '100%').append("<div class='ui-slider-handle'></div>");
		      $(".ui-slider-1").slider({steps: elements.length*settings.nbelem, start: 0, slide: function(e,ui) {
				    mousewheelposition = (elements.length * ui.value /100);
				    caroussel_portfolio_vue(mousewheelposition, portfolio, elements, settings, ratio_largeur, true);
		      }});
		      $(".portfolio-container").mousewheel(function(event, delta) {
						      if (delta > 0) { mousewheelposition+=.3; if(mousewheelposition>elements.length) mousewheelposition = elements.length;
						      } else if (delta < 0) { mousewheelposition-=.3; if(mousewheelposition<0) mousewheelposition = 0;
						      }
						      caroussel_portfolio_vue(mousewheelposition, portfolio, elements, settings, ratio_largeur, false);
						      
						      return false;
		      }).keypress(function(event) {  
			      if (event.keyCode == '9') {
				      return false;
			      } else if (event.keyCode == '37') {
				      mousewheelposition-=.3; if(mousewheelposition<0) mousewheelposition = 0;
				      caroussel_portfolio_vue(mousewheelposition, portfolio, elements, settings, ratio_largeur, false);
				      
						     
			      } else if (event.keyCode == '39') {
				      mousewheelposition+=.3; if(mousewheelposition>elements.length) mousewheelposition = elements.length;
				      caroussel_portfolio_vue(mousewheelposition, portfolio, elements, settings, ratio_largeur, false);	     
			      } 
		      });
		}
		
		$(".portfolio-img a").attr('rel','gallery-'+rel_id).fancybox({'onStart' : function() {$('.flv-type').css('visibility','hidden');}, 'onClosed': function(){caroussel_portfolio_vue(mousewheelposition, portfolio, elements, settings, ratio_largeur, false);}});
		
	});
};

function caroussel_portfolio_vue(current, portfolio, elements, settings, ratio_largeur, bslider) {
	 
	var decalage = parseInt(settings.width/settings.nbelem*current*ratio_largeur);
	for ( var i = 0; i < elements.length; i++ ) {
		$(elements[i]).find('.flv-type, .mp3-type, iframe').each(function() {
			if ( (((i*settings.width/settings.nbelem)-parseInt(decalage)) < 0) || (((i*settings.width/settings.nbelem)-parseInt(decalage)) > (settings.width-settings.width/settings.nbelem + 26)) ) {
				$(this).css('visibility','hidden');
			} else {
				$(this).css('visibility','visible');}
		});
	}
	portfolio.css('left',(-decalage)+'px');
	if (!bslider) $('.ui-slider-handle').css('left', parseInt((current/elements.length)*100)+'%');
};
})(jQuery);


