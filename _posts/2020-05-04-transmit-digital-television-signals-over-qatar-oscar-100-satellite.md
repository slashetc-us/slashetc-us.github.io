---
layout: post
title: Transmit digital television signals over Qatar Oscar 100 satellite
date: 2020-05-04 14:38:04 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

![](https://qrznow.com/wp-content/uploads/2020/05/Transmit_DATV_over_QO100_with_Adalm_Pluto_and-F5OEO_firmware.jpg){sizes="(max-width: 768px) 100vw, 768px" srcset="https://qrznow.com/wp-content/uploads/2020/05/Transmit_DATV_over_QO100_with_Adalm_Pluto_and-F5OEO_firmware.jpg 768w, https://qrznow.com/wp-content/uploads/2020/05/Transmit_DATV_over_QO100_with_Adalm_Pluto_and-F5OEO_firmware-300x169.jpg 300w"}

Intro
-----

Following my previous article on the [transmission to the Qatar Oscar 100 satellite with SDR technology (LimeSDR, Adalm pluto)](https://www.f5uii.net/transmit-over-qo100-eshail2-oscar-satellite-sdrconsole-limesdr-adalm-pluto/) which was dedicated to the narrow-band transponder transmission (SSB and modes with spectral width less than 2.7 kHz), here is an article that should this time interest those who are willing to embark on the digital amateur television program, commonly called DATV (Digital Amateur Television).

As you may have read so far, I have a LimeSDR-mini but now also an Adalm Pluto. During one of my conferences (in french -- see [Amsat Francophone Conference](https://www.f5uii.net/en/actualites/conference-amsat-francophone-qo100-sdr-07-03-2020/)), I had the occasion to compare the two equipments, their advantages and disadvantages. I had the experience to transmit in DATV to QO100 with both the LimeSDR-mini and the Adalm Pluto. Both models work very well, but I will focus in this article on the use of the Adalm Pluto, because finally it is today the solution that is the easiest to implement. Indeed, to transmit with LimeSDR, a Raspberry Pi is necessary. In the case of the Adalm Pluto and especially thanks to the developments of a specific firmware of Evariste F5OEO, we do without this nanocomputer, the Adalm rather embedding the software directly.

In this article, as usual, we will go step by step through all the operations described as precisely as possible.

I use the Windows operating system. I won't go into the details of the operation of the transmission modes themselves, not being a specialist in the fundamentals.  My goal is to bring to your knowledge all the steps that will allow you to build your own transmitting station, and transmit images. It is possible that the software evolving, certain functions, or operations themselves are to be made evolve. Do not hesitate to submit your questions, problems of implementation directly in comment of the article.

Here is the diagram of principle which we will implement, to transmit our DATV videos to the QO-100 satellite.

[![](https://qrznow.com/wp-content/uploads/2020/05/img_5ea575d3f3e5e.png){sizes="(max-width: 714px) 100vw, 714px" srcset="https://qrznow.com/wp-content/uploads/2020/05/img_5ea575d3f3e5e.png 714w, https://qrznow.com/wp-content/uploads/2020/05/img_5ea575d3f3e5e-300x150.png 300w"}](https://qrznow.com/wp-content/uploads/2020/05/img_5ea575d3f3e5e.png)

Source:[LINK](https://www.f5uii.net/en/transmit-datv-over-qo100-with-sdr-adalm-pluto-f5oeo-plutodvb/)











[]{.underline}













[Equipment](https://qrznow.com/category/equipment/)
---------------------------------------------------







[![Ailunce HS2 HF VHF UHF SDR Transceiver](https://qrznow.com/wp-content/uploads/2020/05/maxresdefault-1-1-265x168.jpg)](https://qrznow.com/ailunce-hs2-hf-vhf-uhf-sdr-transceiver-2/)





### [Ailunce HS2 HF VHF UHF SDR Transceiver](https://qrznow.com/ailunce-hs2-hf-vhf-uhf-sdr-transceiver-2/)



[May 05, 2020]{datetime="2020-05-05T10:14:33+00:00"} [No comments](https://qrznow.com/ailunce-hs2-hf-vhf-uhf-sdr-transceiver-2/#respond)



HS2 is an ultra-portable full-frequency full-mode SDR radio. Receive frequency coverage 300K\~1.6GHZ. The built-in network port can realize remote oper\... [Read more ](https://qrznow.com/ailunce-hs2-hf-vhf-uhf-sdr-transceiver-2/)





[![Martin introduces two products from the BHi Range](https://qrznow.com/wp-content/uploads/2020/05/eq20-dsp-265x168.jpg)](https://qrznow.com/martin-introduces-two-products-from-the-bhi-range/)





### [Martin introduces two products from the BHi Range](https://qrznow.com/martin-introduces-two-products-from-the-bhi-range/)



[May 02, 2020]{datetime="2020-05-02T14:48:06+00:00"} [No comments](https://qrznow.com/martin-introduces-two-products-from-the-bhi-range/#respond)



20W Modular Audio Power Amplifier  -- Parametric Equaliser  -- Optional BHi Dual Channel DSP Noise Cancelling and Bluetooth technology. Flex\... [Read more ](https://qrznow.com/martin-introduces-two-products-from-the-bhi-range/)





[![Yaesu on April 27, 2020, introduced a network remote control system LAN unit "SCU-LAN10"](https://qrznow.com/wp-content/uploads/2020/04/SCULAN10-1-265x168.jpg)](https://qrznow.com/yaesu-on-april-27-2020-introduced-a-network-remote-control-system-lan-unit-scu-lan10/)





### [Yaesu on April 27, 2020, introduced a network remote control system LAN unit "SCU-LAN10"](https://qrznow.com/yaesu-on-april-27-2020-introduced-a-network-remote-control-system-lan-unit-scu-lan10/)



[April 27, 2020]{datetime="2020-04-27T13:49:28+00:00"} [No comments](https://qrznow.com/yaesu-on-april-27-2020-introduced-a-network-remote-control-system-lan-unit-scu-lan10/#respond)



Yaesu Radio Co., Ltd., on April 27, 2020, introduced a network remote control system LAN unit "SCU-LAN10" that enables remote operation by\... [Read more ](https://qrznow.com/yaesu-on-april-27-2020-introduced-a-network-remote-control-system-lan-unit-scu-lan10/)





[![QRPVer Minion SDR First Impressions](https://qrznow.com/wp-content/uploads/2020/04/P90626-161323-700x400-1-265x168.jpg)](https://qrznow.com/qrpver-minion-sdr-first-impressions/)





### [QRPVer Minion SDR First Impressions](https://qrznow.com/qrpver-minion-sdr-first-impressions/)



[April 26, 2020]{datetime="2020-04-26T13:17:55+00:00"} [No comments](https://qrznow.com/qrpver-minion-sdr-first-impressions/#respond)



SSB SDR transceiver is a shortwave, ten band, multi band. QRPver Minion-SDR. This is a compact ten-band shortwave QRP SDR transceiver (1.8, 3.5, 5, 7,\... [Read more ](https://qrznow.com/qrpver-minion-sdr-first-impressions/)





[![Q-31 Part 13 All Boxed Up](https://qrznow.com/wp-content/uploads/2020/04/maxresdefault-4-1-265x168.jpg)](https://qrznow.com/q-31-part-13-all-boxed-up/)





### [Q-31 Part 13 All Boxed Up](https://qrznow.com/q-31-part-13-all-boxed-up/)



[April 19, 2020]{datetime="2020-04-19T16:04:14+00:00"} [No comments](https://qrznow.com/q-31-part-13-all-boxed-up/#respond)



[Read more ](https://qrznow.com/q-31-part-13-all-boxed-up/)











[VHF](https://qrznow.com/category/vhf-2/)
-----------------------------------------







[![News from DARC --- 50 MHz](https://qrznow.com/wp-content/uploads/2020/05/csm_BnetzA-265x168.png)](https://qrznow.com/news-from-darc%e2%80%89-%e2%80%8950-mhz/)





### [News from DARC --- 50 MHz](https://qrznow.com/news-from-darc%e2%80%89-%e2%80%8950-mhz/)



[May 06, 2020]{datetime="2020-05-06T20:04:06+00:00"} [No comments](https://qrznow.com/news-from-darc%e2%80%89-%e2%80%8950-mhz/#respond)



The Federal Network Agency made a preliminary provisional implementation of the WRC-19 results for the 6‑m band in MItteilung 111 from Amtsblatt 08/20\... [Read more ](https://qrznow.com/news-from-darc%e2%80%89-%e2%80%8950-mhz/)





[![Aeronautical Mobile. Contacts 100 Miles + on a 5 watt Handheld](https://qrznow.com/wp-content/uploads/2020/05/maxresdefault-1-265x168.jpg)](https://qrznow.com/aeronautical-mobile-contacts-100-miles-on-a-5-watt-handheld/)





### [Aeronautical Mobile. Contacts 100 Miles + on a 5 watt Handheld](https://qrznow.com/aeronautical-mobile-contacts-100-miles-on-a-5-watt-handheld/)



[May 04, 2020]{datetime="2020-05-04T13:08:59+00:00"} [No comments](https://qrznow.com/aeronautical-mobile-contacts-100-miles-on-a-5-watt-handheld/#respond)



"This weeks show is slightly different. What if we take the Anytone AT D878 UV plus and put it in almost ideal conditions. How far can a 5 watt\... [Read more ](https://qrznow.com/aeronautical-mobile-contacts-100-miles-on-a-5-watt-handheld/)





[![DIY Satellite Antenna DU1AU](https://qrznow.com/wp-content/uploads/2020/05/du1au-satellite-antenna-march-2019-265x168.jpg)](https://qrznow.com/diy-satellite-antenna-du1au/)





### [DIY Satellite Antenna DU1AU](https://qrznow.com/diy-satellite-antenna-du1au/)



[May 02, 2020]{datetime="2020-05-02T21:43:14+00:00"} [No comments](https://qrznow.com/diy-satellite-antenna-du1au/#respond)



A satellite antenna can be made from 3 mm copper or aluminum elements, PVC boom, and some parts you may already have at home. To download the plan (hi\... [Read more ](https://qrznow.com/diy-satellite-antenna-du1au/)





[![An amazing 3866.47 KM on 144MHz SSB between D4VHF and J69DS \[ VIDEO\]](https://qrznow.com/wp-content/uploads/2020/04/UR67lrg-265x168.jpg)](https://qrznow.com/an-amazing-3866-47-km-on-144mhz-ssb-between-d4vhf-and-j69ds-video/)





### [An amazing 3866.47 KM on 144MHz SSB between D4VHF and J69DS \[ VIDEO\]](https://qrznow.com/an-amazing-3866-47-km-on-144mhz-ssb-between-d4vhf-and-j69ds-video/)



[April 15, 2020]{datetime="2020-04-15T15:51:09+00:00"} [No comments](https://qrznow.com/an-amazing-3866-47-km-on-144mhz-ssb-between-d4vhf-and-j69ds-video/#respond)



An amazing 3866.47 KM on SSB between D4VHF and J69DS via \#tropo on \#2m \#vhf band. Maybe a new mode world record? TNX \@FransJr for the effort! \#hamradi\... [Read more ](https://qrznow.com/an-amazing-3866-47-km-on-144mhz-ssb-between-d4vhf-and-j69ds-video/)





[![Icom IC-R30 Digital/Analogue Wideband Communications Receiver](https://qrznow.com/wp-content/uploads/2020/04/93518383_3057559940931646_5961498002227986432_o-265x168.jpg)](https://qrznow.com/icom-ic-r30-digital-analogue-wideband-communications-receiver/)





### [Icom IC-R30 Digital/Analogue Wideband Communications Receiver](https://qrznow.com/icom-ic-r30-digital-analogue-wideband-communications-receiver/)



[April 15, 2020]{datetime="2020-04-15T14:18:13+00:00"} [No comments](https://qrznow.com/icom-ic-r30-digital-analogue-wideband-communications-receiver/#respond)



The IC-R30 is Icom's latest wideband handheld receiver. Not only does it receive over a wide (0.1 to 3304.999 MHz) frequency range in AM, FM, WF\... [Read more ](https://qrznow.com/icom-ic-r30-digital-analogue-wideband-communications-receiver/)



















[]{.underline}













[Antenna](https://qrznow.com/category/ham-radio-antena/)
--------------------------------------------------------







[![Why I LOVE my Antenna Wire Measuring Counter!](https://qrznow.com/wp-content/uploads/2020/05/maxresdefault-265x168.jpg)](https://qrznow.com/why-i-love-my-antenna-wire-measuring-counter/)





### [Why I LOVE my Antenna Wire Measuring Counter!](https://qrznow.com/why-i-love-my-antenna-wire-measuring-counter/)



[May 02, 2020]{datetime="2020-05-02T19:50:18+00:00"} [No comments](https://qrznow.com/why-i-love-my-antenna-wire-measuring-counter/#respond)



This is an amazing machine made by "Sam" in the USA. I thank him for hand-assembling these. I have two now -- and Sam is furnishing m\... [Read more ](https://qrznow.com/why-i-love-my-antenna-wire-measuring-counter/)





[![DX Commander Expedition Portable Antenna](https://qrznow.com/wp-content/uploads/2020/04/expedition4-265x168.jpg)](https://qrznow.com/dx-commander-expedition-portable-antenna/)





### [DX Commander Expedition Portable Antenna](https://qrznow.com/dx-commander-expedition-portable-antenna/)



[April 23, 2020]{datetime="2020-04-23T11:46:59+00:00"} [No comments](https://qrznow.com/dx-commander-expedition-portable-antenna/#respond)



A first look at the Portable version of the world famous DX Commander vertical antenna for Ham Radio. [Read more ](https://qrznow.com/dx-commander-expedition-portable-antenna/)





[![HexBeam SP7IDX Technology assembly](https://qrznow.com/wp-content/uploads/2020/04/e41cu2c9ja-265x168.jpg)](https://qrznow.com/hexbeam-sp7idx-technology-assembly/)





### [HexBeam SP7IDX Technology assembly](https://qrznow.com/hexbeam-sp7idx-technology-assembly/)



[April 15, 2020]{datetime="2020-04-15T13:46:42+00:00"} [No comments](https://qrznow.com/hexbeam-sp7idx-technology-assembly/#respond)



[Read more ](https://qrznow.com/hexbeam-sp7idx-technology-assembly/)





[![How to shorten your wire antenna without cutting it](https://qrznow.com/wp-content/uploads/2020/04/unnamed-2-265x168.jpg)](https://qrznow.com/how-to-shorten-your-wire-antenna-without-cutting-it/)





### [How to shorten your wire antenna without cutting it](https://qrznow.com/how-to-shorten-your-wire-antenna-without-cutting-it/)



[April 14, 2020]{datetime="2020-04-14T16:06:14+00:00"} [No comments](https://qrznow.com/how-to-shorten-your-wire-antenna-without-cutting-it/#respond)



[Read more ](https://qrznow.com/how-to-shorten-your-wire-antenna-without-cutting-it/)





[![Shark HF Hamsticks, American Made, Review And Testing](https://qrznow.com/wp-content/uploads/2020/04/unnamed-1-1-265x168.jpg)](https://qrznow.com/shark-hf-hamsticks-american-made-review-and-testing/)





### [Shark HF Hamsticks, American Made, Review And Testing](https://qrznow.com/shark-hf-hamsticks-american-made-review-and-testing/)



[April 08, 2020]{datetime="2020-04-08T17:37:59+00:00"} [No comments](https://qrznow.com/shark-hf-hamsticks-american-made-review-and-testing/#respond)



[Read more ](https://qrznow.com/shark-hf-hamsticks-american-made-review-and-testing/)











[News](https://qrznow.com/category/news/)
-----------------------------------------







[![May/June Issue of ARRL's On the Air Magazine Now Available](https://qrznow.com/wp-content/uploads/2020/05/OTA-May-June-20-Cvr-265x168.jpg)](https://qrznow.com/may-june-issue-of-arrls-on-the-air-magazine-now-available/)





### [May/June Issue of ARRL's On the Air Magazine Now Available](https://qrznow.com/may-june-issue-of-arrls-on-the-air-magazine-now-available/)



[May 07, 2020]{datetime="2020-05-07T12:34:15+00:00"} [No comments](https://qrznow.com/may-june-issue-of-arrls-on-the-air-magazine-now-available/#respond)



The May/June digital edition of ARRL's On the Air magazine is now available. Read it and other ARRL digital publications by browsing to the ARRL Magaz\... [Read more ](https://qrznow.com/may-june-issue-of-arrls-on-the-air-magazine-now-available/)





[![Special Events Commemorate the End of World War II](https://qrznow.com/wp-content/uploads/2020/05/WW-II-VE-Day-Headline-265x168.jpg)](https://qrznow.com/special-events-commemorate-the-end-of-world-war-ii/)





### [Special Events Commemorate the End of World War II](https://qrznow.com/special-events-commemorate-the-end-of-world-war-ii/)



[May 06, 2020]{datetime="2020-05-06T11:29:00+00:00"} [No comments](https://qrznow.com/special-events-commemorate-the-end-of-world-war-ii/#respond)



Several special event stations are on the air to mark 75 years since the end of World War II. In the UK, GB4VVV ("V for victory"), and G0SFJ will oper\... [Read more ](https://qrznow.com/special-events-commemorate-the-end-of-world-war-ii/)





[![How amateur radio is connecting people during lockdown](https://qrznow.com/wp-content/uploads/2020/05/112148531_c67439db-243f-4ede-8594-903cf1460710-1-265x168.jpg)](https://qrznow.com/how-amateur-radio-is-connecting-people-during-lockdown/)





### [How amateur radio is connecting people during lockdown](https://qrznow.com/how-amateur-radio-is-connecting-people-during-lockdown/)



[May 05, 2020]{datetime="2020-05-05T12:09:45+00:00"} [No comments](https://qrznow.com/how-amateur-radio-is-connecting-people-during-lockdown/#respond)



By Vanessa Pearce  BBC News Photo:BILL ROWLAND Amateur radio use in the UK has seen a "significant" rise during the coronavirus lockdown a\... [Read more ](https://qrznow.com/how-amateur-radio-is-connecting-people-during-lockdown/)





[![FCC Providing Flexibility to Volunteer Examiners in Developing Remote Testing Methods](https://qrznow.com/wp-content/uploads/2020/04/fcc-seal_rgb-on-white-large-265x168.jpg)](https://qrznow.com/fcc-providing-flexibility-to-volunteer-examiners-in-developing-remote-testing-methods-2/)





### [FCC Providing Flexibility to Volunteer Examiners in Developing Remote Testing Methods](https://qrznow.com/fcc-providing-flexibility-to-volunteer-examiners-in-developing-remote-testing-methods-2/)



[May 05, 2020]{datetime="2020-05-05T10:05:04+00:00"} [No comments](https://qrznow.com/fcc-providing-flexibility-to-volunteer-examiners-in-developing-remote-testing-methods-2/#respond)



The FCC has clarified that nothing in its rules prohibits remote amateur radio testing, and no prior approval is needed to conduct remote exam session\... [Read more ](https://qrznow.com/fcc-providing-flexibility-to-volunteer-examiners-in-developing-remote-testing-methods-2/)





[![Today we're celebrating National Weather Observers Day!](https://qrznow.com/wp-content/uploads/2020/05/96020509_10157576595837408_5266219612495675392_n-265x168.jpg)](https://qrznow.com/today-were-celebrating-national-weather-observers-day/)





### [Today we're celebrating National Weather Observers Day!](https://qrznow.com/today-were-celebrating-national-weather-observers-day/)



[May 04, 2020]{datetime="2020-05-04T15:53:25+00:00"} [No comments](https://qrznow.com/today-were-celebrating-national-weather-observers-day/#respond)



Weather Observers, also known as SKYWARN Storm Spotters, help the U.S. National Weather Service (NWS) in reporting severe weather to keep their commun\... [Read more ](https://qrznow.com/today-were-celebrating-national-weather-observers-day/)









The post [Transmit digital television signals over Qatar Oscar 100 satellite](https://qrznow.com/transmit-digital-television-signals-over-qatar-oscar-100-satellite/) appeared first on [QRZ NOW - Ham Radio News](https://qrznow.com).

[Article Source](https://qrznow.com/transmit-digital-television-signals-over-qatar-oscar-100-satellite/).
{: .center-text}

