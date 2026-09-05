
# 2.4 GHz Antenna

I started out with a goal of making a not-too-large directional antenna that could be used for a portable RF scanner.  Since 2.4 GHz is so common - there are many RF sources, low-cost tools, etc. it seemed like a good choice. In addition, the wavelength didn't require the PCB (antenna) size to be prohibitively large.

A yagi-uda design was attractive to me, since there's tons of documentation available about design techniques. I particularly liked (Youtube) IMSAIGuy's videos about PCB yagis and how he went about designing his own....even if his wasn't a "traditional" PCB.


## Ansys Electronics Desktop Student

I was interested in using Ansys' student version of High Frequency Structure Simulator (HFSS) to develop the antenna. Even though I appreciate how valuable MATLAB is for so many design and analysis applications, I could not get a solid grasp of what workflow I should use for my particular design.  After watching a few tutorials on HFSS, I quickly learned the process for developing a basic antenna. 

## Base Design

Texas Instruments has an application note DN034 which describes how to design a 2.4 GHz Yagi PCB, which gives exact dimensions. For better or for worse, I objected to making an antenna on a 4-layer board. I had heard about very effective 2-layer yagi designs. I was tempted to deviate from the design a bit. 

The basic concept of a Yagi-Uda is using a driven element (typically a dipole), with a reflector behind it, and one or many directors in front of it. There are a number of tips and tricks to use, rules of thumb if you will about their design. Throughout my life, I've found rules of thumb to be very helpful, especially for explaining concepts off the cuff, in meetings, or when just learning something.  In my opinion, a true professional knows when the rules of thumb fall apart.  Being particularly new to antenna design, my thought process was to try rules of thumb implemented in Ansys and simulate, and then deviate if necessary. 

Looking at some IEEE articles on PCB design, there were some philosophies which conflicted with the TI app note. Of note were variable director spacing. The TI method used a fixed distance, while some other publishers used varying lengths. 

<img width="410" height="262" alt="image" src="https://github.com/user-attachments/assets/fb76f550-5691-45d9-9680-815fc7344274" />

Source: Yagi-uda.com

Given what I wanted to get out of my RF Scanner project, some of my design goals were:

1. High gain (subjective, but I would compare what other designs reported / claimed).
2. High front to back ratio.
3. S11 return loss of <= 14 dB between 2.4 and 2.5 GHz (or VSWR <= 1.5). This doesn't matter so much for my RF Scanner project, but I also wanted the ability to be used as a transmit antenna also.
4. Total board length of <= 140mm (non-negotiable).
5. 2-Layer board, using FR4, to save costs.

## Design Process

My general idea for developing the antenna was to:
1. Use the TI app note as a reference and design off of that.
2. Experiment with several "Rules of thumb", and simulate.
3. Experiment however I felt, and try to notice any patterns.
4. Iterate until I was happy with the simulation results and then adapt the design onto a KiCAD design.

Several challenges I had:
1. The student version of Ansys has a limit on the number of meshes allowed; this limits the complexity of the project.  Several ways I got around this was reducing the mesh quality, a. carefully reducing the size of the radiation boundary; b. changing the radiation boundary from a spherical model to a rectangular one, c. using rectangular-prism modeled vias instead of cylindrical ones, d. limiting the number of vias modeled.

2. On the bottom side of the TI app note board example, a microstrip balun (below) is utilized.
The purpose of the balun is to convert the single-ended, unbalanced 50 ohm coplanar waveguide / ground plane side to a differential "matched" dipole driven element on the other side.
The dimensions of this balun would have to be experimented with in Ansys. The reason being, TI used a dielectric (FR4) thickness of .226mm between the top and bottom copper layers. I ended up deviating from the typical 1.6mm PCB thickness to just 0.8 mm and thus around a ~.751mm FR4 thickness.

A

<img width="1418" height="678" alt="image" src="https://github.com/user-attachments/assets/f194ca55-227a-4e45-b36b-942438d5e93e" />

Source: R. Wallace & S. Dunbar of Texas Instruments, Application note DN034, "2.4 GHz PCB yagi antenna".


<img width="1028" height="337" alt="Screenshot 2026-08-01 111844" src="https://github.com/user-attachments/assets/cb050696-be59-422f-9bd5-7b421772d0c7" />

50 ohm CoPlanar Wave Guide (CPWG) with ground, with reduced / rectangular vias and SMA end-launch taper, modeled in Ansys.

<img width="2185" height="1002" alt="image" src="https://github.com/user-attachments/assets/cb36b0d0-bc71-4d2e-84af-e8b269cd82c7" />

Design iteration and model.

<img width="985" height="497" alt="Screenshot 2026-07-26 144813" src="https://github.com/user-attachments/assets/e941dba8-7bb2-4354-bf08-13f139796e69" />

Simulation of altering one design parameter, such as dipole length. This is showing the real (resistance) and imaginary (reactance) Z-Parameter of S11.

<img width="942" height="447" alt="image" src="https://github.com/user-attachments/assets/0a2a5450-cf66-45f8-9e81-4c89c74d9a25" />

Optimization in Ansys HFSS allows the machine to run many simulations (100 at a time in my case) by altering user-defined parameters such as dipole trace width, dipole to reflector spacing, spacing between the dipole and directors, individual director lengths and spacing, etc. You set design priorities with different weights.


