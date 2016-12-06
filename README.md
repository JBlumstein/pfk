# Are Last.FM Playcounts Affected by Pitchfork Scores?:<br>A Data Analysis

<hr>

<h2>Introduction</h2>

<p>Pitchfork is a music review website that began in the late 1990s, famous for its witty album reviews (scored from 0.0 to 10.0 by 0.1 increments) and influence among serious music fans.

<p>But just how influential is Pitchfork score on album popularity really?

<p>Using review data for 16,000 albums scraped from Pitchfork, and additional album information pulled from the API provided by Last.FM (a music data service that tracks online music streaming), I ran a data analysis to explore this question. The answer is, as expected, "it depends," with "it" referring to genre, and what part of the 0.0-10.0 rating range.

<h2>First things first</h2>

<p>The first step to an analysis is obtaining the data. Prior to a recent reorganization of the Pitchfork site, a list of review index pages was presented in semantic, consistent HTML and CSS. I built a scraper (<a href="https://gist.github.com/JBlumstein/d475f62e686e0d55ab56dae1a0503211">script on gist</a>), to get review data, including artist, album name, review date, and review scores for all album reviews between early 2001 and early 2016.</p>

<p>I then ran the Pitchfork album review data through the Last.FM API (<a href="https://gist.github.com/JBlumstein/2d8d69652ad34f9a9c6766ceb3fc4630">script on gist</a>) to build a larger dataset that includes album level data, such as playcount and genres.</p>

<p>I decided to focus on the effect of Pitchfork rating on Last.FM playcount in the context of time (measured by review date) and genre. Including genre forced me to narrow down the dataset to the ~10K albums out of 16K+ that included genre information. The reviews are dated between 2002 to 2016. I was able to get genre tags from Last.FM. Because there was a huge number of genre tags given to albums on that site, I aggregated the 50 most popular genres for Pitchfork-reviewed albums on Last.FM into 9 "genre clusters" (urban, classic rock, traditional, electronic, indie, alternative, pop, punk, and experimental).

<p>Additionally, I logged Last.FM playcount, leading to a decently even distribution in the response variable, albeit with a long right hand tail, representing albums that garnered few listens on Last.FM.

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/log_plays_per_day_genres_inc.png">

<p>Starting with a simple regression, an ANCOVA (an ANOVA with one or more continuous variables), shows that after factoring out genre and date of album release, a 1 point increase in Pitchfork review score has a +11% effect on Last.FM playcount (a 1% increase in rating per 0.1 point increase in score). It also finds that an album release date one year later has a +20% effect on Last.FM plays per day and that genre has a large effect on Last.FM playcount (e.g., a pop album can be expected to have 8 times as many Last.FM plays than an experimental album). As a result, genre and time should be kept in mind in further examinations of the effect of Pitchfork rating on Last.FM listens.

<br>

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/anova_results.jpg">
<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/change_in_plays.jpg">

<br>

<h2>Digging into Pitchfork Ratings</h2>

<p>Digging into Pitchfork rating over time shows that the bulk of ratings are between 5.0-9.5, with a long left tail from 5.0 down to 0.0.

<br>

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/dists.png">

<br>

<p>Adding to the left tail weirdness, the fatness of the left tail has decreased significantly over time, as Pitchfork has become less eager to give reviews below 5.0. The right tail has tightened somewhat as well. Despite these changes, the mean rating on Pitchfork has stayed fairly steady from 2002 to 2016.

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/pfkquantiles.png">

<br>

<p>So what kind of albums are getting sub-5.0 reviews from Pitchfork? Surprisingly popular ones.

<p>Looking at the average playcount for rolling 1.0 point intervals by genre shows that the rating ranges with the highest mean listenership are far less well regarded by Pitchfork reviewers than the average reviewed album (these charts looks quite similar when the time variable is included, so I've taken it out here for the sake of interpretability).

<br>

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/rolling_playcounts_per_rating.png">

<br>

<p>However, it's not hard to understand this phenomenon: Pitchfork will only bother to review a bad or mediocre album if it's by a well known artist. There's no real impetus for Pitchfork to focus any attention on albums that are both bad and obscure.

<h2>Examining the right edge of the distribution</h2>

<p>It appears that right place to focus is on the right hand side of the distribution. It seems that for at least some genres, albums with a score above 8.0 are getting a boost in Last.FM playcount, and it's worth examining this trend more closely.

<p>A simple regression of logged daily playcount as a function of Pitchfork rating shows that for the urban, classic rock, traditional, indie, and experimental genre clusters there is a statistically significant relationship between Pitchfork rating and logged daily Last.FM playcount.

<br>

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/eight_plus_by_genre.png">

<br>

<p>Classic rock shows the greatest effect of Pitchfork rating on playcount when rating is >=8.0 ( (10^.9198)^.1 = 24% additional plays per 0.1 point increase in Pitchfork rating). This is most likely explained by the large number of reissues in this genre cluster. Reissues reviewed are much more likely than new albums to receive very high Pitchfork ratings (if the album is re-issued, and Pitchfork reviews the re-issue, it's probably a very good album).

<p>For the other genre clusters where Pitchfork rating has a statistically significant effect on playcount when rating is >=8.0, the differences are smaller. For the traditional cluster, a 0.1 point increase in Pitchfork rating when rating is >=8.0 leads to 13% additional plays. For the urban, indie, and experimental genres, a 0.1 point increase above >=8.0 leads to 9% more plays.

<p>The effect is more powerful when log plays per day is adjusted by time of release. In this analysis, albums with an earlier release date are assigned additional plays based on a multiple regression of log plays per day as a result of date of release and rating and then subtracting the effects of date of release from each side to get log plays per day adjusted by date of release as a function of Pitchfork rating. Now 7 of the 9 genres show rating having a statistically significant effect on the response variable (including alternative and electronic, which did not have statistically significant results before). The recorded effects are also generally larger (~12% increases in playcount for each additional point in rating vs. ~10% increases in playcount for each additional 0.1 point in rating when unadjusted).

<br>

<img src="https://raw.githubusercontent.com/JBlumstein/myassets/master/regress_results_time_adj.png">

<br>

<h2>Conclusions</h2>

<p>While Pitchfork rating is not as big an influence on Last.FM daily playcount as genre or date of release, for a majority of genres there is a statistically significant bump in playcount resulting from increases in Pitchfork rating when at the right hand side of the distribution. There appears to be on average an ~10% increase in Last.FM playcount, and a ~12% in Last.FM playcount adjusted by date of release, per 0.1 increase in Pitchfork rating. This bump varies by genre.
