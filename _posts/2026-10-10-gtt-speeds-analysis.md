---
layout: post
title: "Evaluating the average speed of public transport in Turin"
date: 2026-10-09 12:00:00
description: "So much work"

---

I have written in the past about my collection effort for the real-time positions of public transport vehicles in Turin (see <a href="{% post_url 2026-07-20-torino-bus-analysis %}" target="#"></a> for some details about the GTFS Realtime data). I have already analyzed the GTFS RT data for a single line in 2024, showing the average speeds of a bus route during the summer break, in <a href="{% post_url 2024-09-18-torino-bus-ago-m1s-it %}" target="#">this post</a> (in Italian). 
Months before that, I started to explore other real-time vehicle location data, but still only for a single line and a single month (see <a href="{% post_url 2024-02-01-torino-bus %}" target="#">this post</a>). The results were interesting but they were difficult to interpret. A more systemic analysis was then needed, with a stronger methodology, to compare the speeds of different lines across different months.

So I looked back at the code I used for the previous analysis of GTFS RT positions, which has been made in Python. Over the months, I rewrote a lot of the code to be able to parse the GTFS RT positions of any month, while specifying the line and the direction to use. This intermediate step, matching a vehicle with the correct direction, has been possible (until today) because of the trip information given with the location update. Therefore, for each vehicle I was able to obtain the path it was supposed to take (`shapes` in the GTFS static data) and the stops it was servicing. 

### Troubleshooting and definitions

One issue with the analysis in <a href="{% post_url 2024-02-01-torino-bus %}">the 2024 post</a> is that the sections did not start and end at different stops. It is not a big one, but if the sections are small in length, then I might get a stop in the middle of it when the bus is always stopping at, thus artificially lowering the speed measurement. So, I decided to start and end the sections for measuring the average speed at different stops. 

Secondly, it is important to define correctly the way to measure the speed of a vehicle. If there are two consecutive GPS positions inside a single section, I can then define a speed measurement which is the distance between the two points divided by the time difference between them, but (a) the GPS measurement might be off by some meters and (b) if I do averages of several of these speed measurements inside one section I get (as close as it is possible) an average *instantaneous* speed. This is informative, but not what I intended to measure. 

In the end, I wanted to get the average speed of a line taking from one stop to the next (or even further), which is actually the *commercial* speed. 
To achieve this, I defined speed measurements for each time the vehicle crosses a threshold (a stop position on the path of the trip), by dividing the distance between that and the previous threshold / stop by the time it took to move from the previous to the current one. For this work, I have computed the distance between stops on the ideal (scheduled) path, finding for each consecutive GPS position of a vehicle the corresponding location on the path. 

This method has the benefit of checking vehicles keep the correct path (within margin of course), but it took many trials to get the correct code for finding the positions: when a scheduled path loops back within a small distance, it can introduce bugs in the definition of the sections from the stops, or the tracking of a vehicle along the line. Ironing out this kind of bugs took some time, and I am quite confident that now they have been squashed.

## Onto the results then!

As you might know, I have collected several months of realtime bus locations data over the years. I decided to focus on the months with the best coverage of working days (you can see from the dashboard linked in <a href="{% post_url 2026-07-20-torino-bus-analysis %}">my last post</a> that in some months there are gaps, or data quality issues), starting with February 2026. I choose to concentrate on working days because (a) this is when the public transport is more needed and (b) because often on weekends there were issues with the web service for the realtime vehicle positions.

I also wanted to show how the average speed changes throughout the day, so I separated the speed measurement of each vehicle and section by the hour at which it was taken. 

Below, you can see the average speeds at different times during the day for all the **tram lines** in February 2026:
{% include image.html path="assets/img/post_speeds/speedplot_times_trams_2026_02.png" class="img-fluid rounded z-depth-1" figclass="text-center" %}

From this figure, it is possible to observe how the speed increases significantly during the night hours for almost all line sections shown. The city center in particular shows quite red during peak hours, and in general does not show very high average speeds (max 17 km/h during the night).
There are instead a few section which remain a constant colour, in particular the one in the bottom left corner: this is where the line 10 has completely separated track from normal traffic, with almost no traffic lights. 
A similar reasoning can explain other sections with higher average speed, as in line 9, where trams are completely separated from private traffic in the sections in the south end:

{% include image.html path="assets/img/post_speeds/speedplot_2026_02_line_9_dir_0_manyh.png" class="img-fluid rounded z-depth-1" figclass="text-center" %}
Above the speeds are shown for the north-bound direction, below for the south-bound:
{% include image.html path="assets/img/post_speeds/speedplot_2026_02_line_9_dir_1_manyh.png" class="img-fluid rounded z-depth-1" figclass="text-center" %}

One insight that is possible to glean form plots like this is that that whenever the path turns, the speeds gets much lower: this is visible in the above plot when the path goes almost to the left border, turning south-east.

Some lines are much more affected by private traffic during the day, like line 2:
{% include image.html path="assets/img/post_speeds/speedplot_2026_02_line_2_dir_1_manyh.png" class="img-fluid rounded z-depth-1" figclass="text-center" %}

In order to make the results interactive, I have created yet [another dashboard](https://gttspeeds.streamlit.app/) to display them. In addition to seeing the average speed in small sections, with the dashboard it is possible to change the minimum size of the sections and therefore perform measurements of commercial speeds over larger distances. 

You can see one of the available visualizations below, showing the average speed for sections of (minimum) 500 meters at every hour during the day. This way, one can easily observe the changes in average speed, and highlight particularly slow sections.
 <div class="mt-0 mt-md-0 mx-auto float-end"  style="max-width:800px">
        {% include image.html path="assets/img/post_speeds/heatmap.png" title="Line sections and trace"  class="float-end img-fluid rounded z-depth-1" %}
    </div>

I could show more results, but the main conclusions remain. Let's elaborate them on them now:
1. There is a significant decrease an average speed whenever a vehicle has to turn instead of going straight. This might be due in part by physics (slowing down is good before making a turn!) but it is completely possible that a vehicle has to wait more at the traffic light to get the correct one to turn. In some crossings with large roads (like the one I showed above for line 9) there is often significant private traffic flow, which may be prioritized by keeping the light green for going straight.
2. All this analysis shows large evidence that separating the public transport vehicles with a reserved lane helps a lot (duh!!). If you repeat the plot right above (line 2, speeds at different hours for sections of 500 m) for the month of September 2026 instead of February, you will find that the in some sections (from CASTELDELFINO to REBAUDENGO) the speeds are much higher, placing those sections right on average with other ones. Might that be because of new dedicated lanes to public transport?
3. Finally, the average commercial speeds for lines traveling in or near the city center (up until the inner ring of trams shown in the first plot) are quite low (from 10 to 12 km/h). This happens even when there are dedicated lanes, and there are many factors accounting for this: the roads are narrow, there are many turns, the flow of public transport vehicles increases too... The only (serious) solution is to put heavy lines, like the trams, underground - or build a new underground line altogether. The latter is the choice that has been taken, and it is about to happen soon, right?

### Conclusions

This analysis was due for a long time, and it has been very much fun to do. Along the way, even though it took a lot of time to properly do all the project, I acquired a lot of experience in dealing with geo-spatial data, and I quite happy at how it turned out.

Feel free to explore the visualizations available in my dashboard at [gttspeeds.streamlit.app](https://gttspeeds.streamlit.app/), if you see something strange or noteworthy, please tell me!

Also, the first visualization I've shown was inspired by a presentation done by the [Velocipiedi](https://velocipiedi.it/) team in Milan. They have made almost the same analysis, but since there are no publicly available endpoints to download the location of the buses in real time, they had to it by had using their phones! 