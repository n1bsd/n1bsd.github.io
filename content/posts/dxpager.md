+++
slug = 'dxpager'
title = 'DXPager'
date = 2022-07-21T00:00:00+00:00
draft = false
tags = ["Ham Radio", "Python", "Software"]
+++
I just released DXPager, a script that sends DAPNET messages to your pager if a DX station whose entity you have not worked/confirmed before has been spotted. In other words: The script checks your LotW QSL log, observes the DX cluster and pages you as soon as a new one appears. A "(L)" is added to the message if the DX station is a LotW user. Additionally, you can define a watch list of DX call signs for which you'd also liek to be notified.


![](/img/dxpager-1.jpg)


What it does in more detail:

  * parses the output of a specific dx cluster server
  * downloads your LotW QSL file
  * determines the DX station's country
  * determines the DX station's continent
  * determines if the DX station uses LotW
  * determines if the DX station's country has been confirmed via LotW
  * checks if the DX call is in your VIP call list
  * de-duplicates spots
  * and finally - if it's a new DXCC or a watchlist triggered - sends the information to your DAPNET pager


# Code

You can find the code and some more information [here](/files/dxpager.tar.gz).
