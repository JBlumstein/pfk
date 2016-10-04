# Are Last.FM Playcounts Affected by Pitchfork Scores?:<br>A Data Analysis

<p>Pitchfork is a music review website that began in the late 1990s, famous for its witty album reviews (scored from 0.0 to 10.0 by 0.1 increments) and influence among serious music fans.</p>

<p>But just how influential is it really?</p>

<p>Using album review data scraped from Pitchfork and pulled from the API provided by Last.FM (a music data service that tracks online music streaming), I ran a data analysis to explore this question. Using ANCOVA (an ANOVA with one or more continuous variables), I found that after factoring out genre and date of album release, a 1 point increase in Pitchfork review score has a +11% effect on Last.FM playcount. I also found that an album release date one year later has a +20% effect on Last.FM plays per day and that genre has a large effect on Last.FM playcount (e.g., a pop album can be expected to have 8 times as many Last.FM plays than an experimental album).

<p>One additional intersting finding was that while the mean rating on Pitchfork has stayed steady from 2002 to 2016, the distribution of ratings has narrowed greatly. In particular, Pitchfork is not nearly as eager to give ratings below 5.0 as it used to be.

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/pitchforkquantilesovertime.png">

<p>In this repo are the analysis (the Jupyter notebook) and the data set (the CSV).</p>

<p>Enjoy!</p>
