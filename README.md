#Research on ABR Algorithms performance

Abstract – This study explores the three Adaptive
Bitrate Algorithms (ABR) implemented over the
dash.js media player. These three algorithms are
the Throughput ABR, BOLA ABR, and Dynamic
ABR. These three algorithms work to bring to the
user the highest quality video stream while
keeping the playthrough buffer free. Throughput
ABR dynamically selects the bitrate depending on
the network throughput. BOLA ABR selects the
bitrate depending on the current buffer level.
Lastly, Dynamic ABR selects the bitrate by
looking at the entire network condition, it is like a
hybrid between Throughput and BOLA ABR. We
ran tests to observe how each of these algorithms
performs under a controlled unstable
environment. The results are indicative of how
important it is to select the right ABR algorithm
for your specific network condition. Throughput
works best under a strong network, BOLA works
best in an unstable network, wheras Dynamic
adapts to both.
I. Introduction
User satisfaction is the most important aspect when
it comes to a successful product. The same applies to
online video streaming. Delivering a seamless, highquality video streaming experience to users on
different networks of varying speeds and reliability
can be challenging. Adaptive streaming over HTTP
(DASH) tries to solve this issue by dividing videos
into segments of equal size and encoding them at
different bitrates which the user-client will then
select depending on their network every few
seconds. Selecting these encoded segments to
download will be decided following one of the three
Adaptive Bitrate algorithms (ABR). Selecting the
right ABR can have a big impact on the viewing
enjoyment of online video streams. Understanding
their differences and strengths becomes imperative.
The three adaptive bitrate algorithms are Dynamic,
BOLA, and throughput. Buffer-based Optimized link
Adaption (BOLA) [1] dynamically changes the
quality of the video stream based on the available
buffer level, its estimation of future buffer
occupancy, and selecting the appropriate bitrate for
the next segment. Throughput-based Adaptive
Bitrate employs a different approach to dynamically
adjusting the quality of the video stream.
Throughput ABR dynamically adjusts the quality of
the video stream depending on the network
throughput conditions [1]. Depending on the
throughput, the algorithm will select the video
quality. A higher throughput would allow for more
bitrate which in turn would allow for a better-quality
segment. Lastly, Dynamic Adaptive Bitrate employs
a mix of the two. Dynamic Adaptive Bitrate aims to
dynamically change the quality of the video stream
depending on the network conditions and the buffer
level. In this report we aim to get a better
understanding of these three ABR strategies. To do
so, we ran a media player application based on
dash.js that played a stored (VoD) DASH playlist
and allowed us to choose which ABR algorithm we
wanted to run. To recreate different users network
conditions and tests the ABR algorithms adaptability
to unstable networks, we also used Chromes built in
throttling strategies, specifically Fast3G and No
Throttling which we ran sequentially for a given
time. Lastly, to collect data and compare the various
ABR algorithm, we implemented a JavaScript script
to collect the selected bitrate (Mbps), buffer level
(second), measured throughput (Mbps), segment
download time (second), and segment size (byte)
every 8 seconds and store them into a .csv file.
II. Collecting Data
Before we start we need to explain a certain issue
with the collected data. When we initially start the
collection, and sometimes throughout the collection,
on Fast3G, a jumping gap occurs which delays the
built in timer by a little bit. This can be seen as the
first packet we collect starts at 9 seconds and not 8. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/d0cf0c1e-6013-4948-9e55-af55c5566c16)

These slight changes in time collection are not
severe or big enough to impact the validity of the
data we collect. To perform these test, we
implemented a small script into the main.js of the
dash.js website that retrieves the data we needed. To
do so, we used the $scope object that was part of the
controller. This object contained all the information,
such as bitrate and buffer, that we needed. The data
collection was built into the updateMetrics()
function which we modified to be updated every 8
seconds.
III. Results
For all the table, the tables will be a condensed
version of the results, meaning that some of the rows
will be cut out to make the tables shorter. For each
table we will mark the switches from Fast3G to No
Throttle and the reverse as goes: We mark that at
time 177 seconds, the switch was made from Fast3G
to No Throttle, and that at 337 seconds, the change
was made from No Throttle to Fast3G and then
again from Fast3G to No Throttle at 481 seconds.
We will begin with the results for the ABR
Throughput. The table below (T.1) represents the
data collected after running the video stream with
Throughput ABR.

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/1673edab-7b45-4a0e-a8a9-9f17eb544844)

Next up, we will be looking at the BOLA ABR. The
table below (T.2) represents the data collected after
running the video stream with BOLA ABR. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/92767ebb-ced5-4dfe-9a7a-cf772c315815)

Lastly, we will be looking at Dynamic ABR. The
table below (T.3) represents the data collected after
running the video stream with Dynamic ABR. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/c1c24207-9b73-4c49-8a6e-44f6386d8fb9)

IV. Comparison
Looking at the results we can notice a few
discrepancies between the different ABR algorithms
used. Comparing the results, we notice a few key
differences between the algorithms.
Starting with Throughput ABR. It had the clearest
adjustment of bitrate. As soon as the throttle was
switched from Fast3G to No Throttle, the bitrate
went 0.882 Mbps to 1.628 Mbps and back down
whenever we switched it back to Fast3G. We also
notice that buffer levels were directly corelated to
the bitrate, where the higher the bitrate the higher the
buffer level. Lastly, we notice that the throughput
was limited to the maximum throughput of Fast3G
which is 1.44 Mbps, but when No Throttle was
turned on, it reached as high as 209 Mbps.
Looking at the BOLA ABR, we can clearly see that
when No Throttle was set the maximum bitrate was
hit and it went back down to the minimum bitrate
after Fast3G was turned on. We can also see that the
bitrate was proportional to the buffer level. As the
buffer level went up, so did the bitrate.
Lastly, looking at the Dynamic ABR, we can see that
it had the least amount of bitrate variation. The first
run of the No Throttle, the bitrate remained the same
whilst the buffer level increased drastically. Only on
the second No Throttle run did the bitrate hit the
maximum value.
Comparing all three we can see that the Throughput
ABR controls the bitrate depending on the
throughput, whereas the BOLA ABR controls the
bitrate depending on the buffer. The Dynamic ABR
has the worst bitrate of the three. We will be
discussing these results in further detail in the
discussion.
V. Discussion
In this section, we discuss the discrepancies between
the results and the expected results, the possible
reasons that could be creating irregularities, and how
the testing could’ve been improved.
A. Throughput ABR: Throughput ABR is
described as an algorithm that controls the
bitrate depending on the network
throughput. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/d7c44ba3-01c1-41db-b768-91596e5bbe5e)

Figure 1: Bitrate over time for Throughput ABR
Looking at the graph (Figure 1) for the bitrate over
time, we can clearly see whenever No Throttle was
turned on, a higher bitrate was enabled. This goes
with the theory. As the throughput increases, the
bitrate and the quality of the video stream increase.
Fast3G has a throughput limit of 1.44 Mbps, this
throughput limit directly limits the bitrate as we can
see on the graph.

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/01c1a183-9cd5-4e16-8c07-2fd829543985)

Figure 1: Throughput over time for Throughput ABR
Following the graph (Figure 2) of the throughput
over time, we can see that the bitrate increase
directly correlates with the throughput increase. Our
tests correctly replicate this scenario, inline with the
theory.
B. BOLA ABR: The Buffer Optimization Link
Adaptation algorithm tries to control the
bitrate proportionally to the buffer level.

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/b8a28b03-3f79-4741-9ef5-630976375dbc)

Figure 3: Bitrate over time for BOLA ABR
Like the Throughput ABR, we can clearly see the
moment where No Throttle was turned on in the
graph (Figure 3). We do notice that during the first
Fast3G run the bitrate went up by a little. This could
be due to a multitude of things, namely poor network
conditions. As we mentioned in the data collection
section, there are moments where a jump is made.
The jump is made because of a possible interruption
or inconsistency in the segment being played. Since
the jump was made with a higher bitrate, BOLA tries
to mitigate this issue by dropping the bitrate and
populate the buffer, which we can see happens in the
graph (Figure 4) below. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/41de6190-3766-4e4f-8ee0-d5ca2383fdc6)

Figure 4: Buffer Level over time for BOLA ABR
As we can see around the time the bitrate went
down, the buffer went up. We can also see that as
soon as the buffer starts increasing drastically, the
bitrate follows. Our tests correctly replicate this
scenario, inline with the theory.
B. Dynamic ABR: Dynamic ABR tries to
implement a mix of both Throughput and
BOLA ABR. It is a hybrid between the two
and takes into consideration the whole
network condition, throughput and buffer
level included. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/1984d300-959b-4b8a-8909-d9a4d1b19544)

Figure 5: Bitrate over time for Dynamic ABR
Looking at the graph (Figure 5) above, we can see
that the Dynamic ABR had the worst bitrate out of
the three ABR algorithms. The Dynamic ABR
algorithm is supposed to look at the complete
network condition and tries to optimize the bitrate.
Looking at the graphs below we can get a sense of
what the algorithm did. 

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/1f1a00f5-91a1-40f8-bd57-abbbd2f4efb5)

Figure 6: Buffer Level over time for Dynamic ABR

![image](https://github.com/Cocovert1/DASH.js-Research/assets/94801567/2b956290-0e84-4741-acda-899915732ccc)

Figure 7: Throughput over time for Dynamic ABR
Looking at the two graphs above (Figure 6 and 7) we
can see that although the throughput went up, the
buffer level went down, indicative of the segments in
the buffer being played. While we were doing the
test for this algorithm, we noticed quite a lot of
latency and buffering. What would explain the low
bitrate would be that the buffer was filled up with
the low bitrate because the network was unstable
before the No Throttle and so the algorithm thought
it was best to download the whole video in a lower
bitrate and slowly download the video in a higher
bitrate after. We can see this happen around the end,
on the second No Throttle, where the bitrate went to
the maximum value. The test are not conclusive
enough to confirm the theory.
All in all, our tests indicate that the Throughput ABR
had the best quality video quality, which makes
sense. The network we were using for No Throttle
was powerful and stable. Having the video quality
being dictated by the throughput was the best option,
which is why it performed the best. BOLA ABR has
the second-best bitrate quality overall. Using the
buffer level to control the bitrate does have its
limitation as BOLA ABR cannot predict future
buffer levels. This can introduce latency and bitrate
drops as the buffer can suddenly be overfilled, as we
can see towards the end of the video where the
buffer level increases dramatically, and the bitrate
had to be lowered. Dynamic has the worst bitrate of
the three overall.
The tests could be improved. For these tests, we
collected our data in incognito mode to not cache the
video beforehand and throttled the network using the
DevTools Fast3G. Although Fast3G is supposed to
simulate an instable Fast 3G network it is not as
good as a real unstable, congested network. One way
to make this test more credible would be to do it in a
real unstable network, which would introduce a lot
more random network conditions.
VI. Conclusion
In conclusion, we discussed the three different
dash.js ABR algorithm used to provide the user with
a stable and high-quality video stream even with an
unstable network. We discussed how the Throughput
ABR algorithm selected the bitrate based on the
network throughput, making it optimal to use in a
perfect network. We then discussed how BOLA ABR
used the buffer level to select the bitrate, allowing
for a smoother transition between bitrates for the
user but limited by the fact that it cannot predict
incoming buffer levels. Lastly, we discussed about
the Dynamic ABR which looked at the complete
network condition to determine the selected bitrate
and how it was limited by the Fast3G throttle. These
algorithms all have their strengths and weaknesses
and work better in specific network situations. In our
case, the Throughput ABR had the best performance
as the network we were using was already very
stable and fast. These tests could’ve been improved
has we used a real unstable network.
VII. References
[1] Anon. 2023. Adaptive bitrate streaming. (August
2023). Retrieved November 14, 2023 from
https://en.wikipedia.org/wiki/Adaptive_bitrate_stre
aming 

