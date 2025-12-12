---
title: "Katz Lab - Technology"
layout: textlay
excerpt: "Katz Lab -- Technology"
sitemap: false
permalink: /technology/
---

# Technology

This is a repository for open source electrophysiology hardware building and data analysis software used by and/or designed in the Katz Lab.

*For any questions, or for further details, please contact [Joseph Wachutka](https://sites.google.com/a/brandeis.edu/katzlab/people/current/joseph-wachutka) or [Narendra Mukherjee](https://narendramukherjee.github.io).*

## Electrophysiology Recording System

We use the RHD2000 family of electrophysiology hardware/software from [Intan Technologies](http://www.intantech.com/). The Intan software and electrophysiology data recording is run/stored on a PC running the Ubuntu operating system. This PC interfaces with a Raspberry Pi 3 running Ubuntu Mate to delivery time-locked experimental information to the RHD2000 interface board via digital input/output channels on the two hardwares.

## Electrophysiology Processing and Data Analysis

Data from our RHD2000 system is stored on a local storage server here at Brandeis University, and analysis is run on the [Jetstream Cloud](http://www.jetstream-cloud.org/) hosted by Indiana University.

## Micro-drive Design and Construction

- Download .stl files below in order to 3D print the body of the micro-drive. We use [Sketchup](http://www.sketchup.com/) to design and edit our designs, but there are many free softwares out there for 3D design and editing (files attached at bottom of page).
- 3D print the top and bottom piece for the micro-drive.
  - We use a [FormLabs Form 2](https://formlabs.com/3d-printers/form-2/) printer and the print quality is very good.
- Glue 1.2M nut into hex recession on 3D printed base piece.
- Place Screw through 3D printed top piece, place nut on screw and tighten until snug (but not tight) against the top piece. Glue or solder the nut to the screw in this position, holding the top piece snug between the top of the screw and the nut.
- Insert the screw into the nut glued into base piece and lower until screw is through the nut.
- Cut the inner cannula (shown in light purple) tube to 20mm in length (this cannula will house the electrode wire).
- Cut the outer cannula (shown in green) to 12mm in length, and insert into bottom piece as shown. Glue in place.
- Insert the inner cannula through small hole in top piece as shown, and through outer cannula. Glue in place to top piece.
- The inner cannula should now move up and down through the outer cannula as you rotate the screw.
  - The length and gauge of these cannulas should be adjusted based on size/number of electrode wires and depth of target implant location.
- Use a caliper to measure the distance of travel per rotation (the screw and nut piece listed here moves 0.25mm per rotation of the screw

## Electrode Design and Construction

- Cut wire into 16 strands of the same length. Wire length depends on the design of the electrode, we cut our wires to ~15-20cm in length for the design described here.
- Bundle these 16 wires together and glue the bottom 5cm of the wire with a liquid glue (this helps for feeding the wire through the micro-drive later).
- Take the top (non-glued) portion of the wires and separate the individual wires. Feed each wire through a holes on the green board (listed in supplies below, SFC attachment shows design of green board)
- Repeat the first 3 steps for the other row on the green board, for a total of 32 wires.
- Remove insulation from the tips of the wires that are contacting the green board using a small razor.
- Place Omnetics connector (listed in supplies) on top of the green board contacts.
- Solder the Omnetics connector to the green board so that one wire is in electrical contact each leg of the Omnetics connector.
  - Trim excess plastic from green board, arms have been added to the design to facilitate building.
- Place the bottom end of the 32 wires (in 2 bundles of 16) through 4cm length of heat-shrink tubing (listed in supplies), slide the tubing as far up the wire as possible and apply heat
- Place the bottom end of the 32 wires through the inner cannula on the micro-drive (design explained above)
  - When enough wire has been fed through the inner cannula so that the heat-shrink tubing comes in contact with the top of the micro-drive, glue the end of the heat-shrink tubing to the micro-drive (be careful not the get glue on the screw).
- Cut the wire that exits the bottom of the inner cannula to length (we use ~3mm exposed wire length).
- Test connectivity of the wire prior to usage.

## Cost Estimation

**Micro-Drive**  
3D printed components $0.15  
Screw $0.05  
Nuts (x2) $0.03  
Cannulas (x2) $0.50  
Glue $0.10  
**Total micro-drive cost ~$0.83**

**Electrode**  
Wire $10.45  
Green Board $1.50  
Solder $0.25  
Glue $0.25  
Omnetics Connector $35.00  
**Total electrode cost ~$47.45**

## Supplies List

### Electrophysiology Recording System

- RHD2000 Evaluation Board: [Intan Link](http://www.intantech.com/RHD2000_evaluation_system.html)
- RHD2132 Headstage Amplifier Boards: [Intan Link](http://intantech.com/RHD2132_RHD2216_amp_board.html)
- Pricing for these Intan products can be found here: [Intan Link](http://www.intantech.com/pricing.html#RHD2000)
- Ubuntu operating system for data storage/electrophysiology data collection and analysis: [Free Download Link](https://www.ubuntu.com/)

### Experimental/Behavioral Control System

- Raspberry Pi 2 or Raspberry Pi 3 boards will work for experimental control: [Raspberry Pi Website](https://www.raspberrypi.org/)
- Raspberry PI 3: [Amazon Link](https://www.amazon.com/Raspberry-Model-A1-2GHz-64-bit-quad-core/dp/B01CD5VC92)
- Ubuntu Mate operating system for Raspberry Pi: [Free Download Link](https://ubuntu-mate.org/raspberry-pi/)

### Microdrive Supplies

- Screws for Katz Lab microdrive: [Amazon Link](https://www.amazon.com/Phillips-Countersunk-Machine-Stainless-100-piece/dp/B06XBTVHFC/ref=sr_1_cc_3?s=aps&srs=3041233011&ie=UTF8&qid=1492527492&sr=8-3-catcorr&keywords=m1.2%2Bscrew&th=1)
- Nuts for Katz Lab microdrive: [Amazon Link](https://www.amazon.com/uxcell-Nickel-Hexagon-Fasteners-1000PCS/dp/B01N3MJ2HC/ref=sr_1_17?s=hi&ie=UTF8&qid=1492527863&sr=1-17&keywords=m1.2%20bolt%20nut)
- Heat-shrink tubing for covering exposed electrode wire: [Amazon Link](https://www.amazon.com/Uxcell-a11110900ux0075-Shrinkable-Shrink-Tubing/dp/B00843KWKS/ref=pd_sim_328_2?_encoding=UTF8&pd_rd_i=B00843KWKS&pd_rd_r=4HG2MPMTAAV8F5YWNG6B&pd_rd_w=4PcZT&pd_rd_wg=9UKhq&psc=1&refRID=4HG2MPMTAAV8F5YWNG6B)
- 32-channel + fiber optotrode outer cannula (0.042" OD, 0.032" ID): [Amazon Link](https://www.amazon.com/dp/B000FN5Q3I/ref=biss_dp_t_asn)
- 32-channel + fiber optotrode inner cannula (0.028" OD, 0.0215" ID): [Amazon Link](https://www.amazon.com/dp/B004UNEXBA/ref=biss_dp_t_asn)
- 32-channel electrode outer cannula (0.03575" OD, 0.02775" ID): [Amazon Link](https://www.amazon.com/dp/B000FN3LJO/ref=biss_dp_t_asn)
- 32-channel electrode inner cannula (0.025" OD, 0.019" ID): [Amazon Link](https://www.amazon.com/dp/B004UN8NKW/ref=biss_dp_t_asn)
- 16-channel mouse outer cannula (0.032" OD, 0.023" ID): [Amazon Link](https://www.amazon.com/dp/B004VS4B7K/ref=biss_dp_t_asn)
- 16-channel mouse inner cannula (0.02" OD, 0.016" ID): [Amazon Link](https://www.amazon.com/dp/B004UNEW7U/ref=biss_dp_t_asn)
- Custom designed plastic drive parts are 3D printed using a [FormLabs Form-2](https://formlabs.com/3d-printers/form-2/) printer. (.stl files attached below, 3 sizes)

### Electrode Supplies

- Nichrome/Formvar electrode wire (either 0.001" or 0.0015" coated diameter): [A-M Systems](https://www.a-msystems.com/s-102-nichrome.aspx)
- 32-channel Omnetics connector: [Part Number A79026-001](http://www.omnetics.com/neuro/nano-strip/), [Specs Sheet](http://www.omnetics.com/neuro/pdffiles/A79026-001.pdf), [Pricing](http://www.omnetics.com/neuro/neuro_type.aspx?type=A79026-001&amp%3Bcategory=nstrip)
- Green Boards used to connect microwires to 32-channel Omnetics connector: Custom made by [SF Circuits](https://www.sfcircuits.com/) (.pdf file of our design is attached below.)
- Low-temp solder paste: [Amazon Link](https://www.amazon.com/Clean-Temperature-Solder-Paste-Grams/dp/B017RSGPI8)
