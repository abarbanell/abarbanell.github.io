---
layout: post
title: "Innovations in Analytics for Academic Publishing "
description: "Conference presentation from the conference Social Media and Web Analytics Innovation, San Francisco, 30-Apr-2015"
category: slides 
tags: [Data Science, presentation, Analytics]
flexslider: 
 - image: /assets/img/2015/04/30/Analytics/Slide01.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide02.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide03.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide04.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide05.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide06.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide07.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide08.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide09.jpg
 - image: /assets/img/2015/04/30/Analytics/Slide10.jpg
---
{% include JB/setup %}


My colleague Sandra Hausmann and I gave a talk on 30-Apr-2015 at the 
conference "Social Media and Web Analytics Innovation" in San Franscisco.


<div class="flexslider">
	<ul class="slides">
		{% for slides in page.flexslider %}
			<li>
				<img " src="{{ slides.image }}">
			</li>
		{% endfor %}
	</ul>					
</div>

you can [get the PDF here]({{ site.url }}/assets/pdf/Analytics.pdf).

