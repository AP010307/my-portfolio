---
title: "Reflow Oven Controller"
date: "2025-03-08T16:46:16-08:00"
draft: false
tags: ["Altium", "Assembly", "Circuit Design", "Python"]
categories: ["Projects"]
github: "https://github.com/harryHatesCPEN211/ELEC291_project_1"
weight: 2

---

![Reflow Oven Schematics](/image/Project1_Schematics.png)


# Overview
I spearheaded a group of five people in an ELEC 291 project that utilized Intel's 8051 microcontroller and assembly language to implement reflow soldering using a toaster oven.

# Introduction
Reflow soldering was used to assemble surface-mounted-technology (SMT) devices onto printed circuit boards (PCBs). The process utilized solder paste (a mixture of solder flux and pellets with adhesive properties) to attach micro-components onto the PCBs. Once components were moderately secured on the pads, the board was placed into an oven, which was gradually heated according to a predetermined soldering profile. This profile included a customizable reflow time, reflow temperature, and soak time. During the heating process, the flux was activated, and the solder paste melted, soldering the components onto the PCB.

# Objective

This project investigated and executed all aspects of reflow soldering: design, hardware, software, testing, and application. The following requirements were strictly followed:

- **Software**  
  - The software was written in Assembly using the 8051 instruction set.  
  - Data validation and plotting were performed in real-time using Python Matplotlib.  

- **Reflow Oven Requirements**  
  - The oven measured temperatures between **25℃ and 240℃** with **±3℃ accuracy**.  

- **User Interface and Feedback**  
  - Customizable parameters via pushbuttons:  
    - Reflow temperature  
    - Reflow time  
    - Soak temperature  
    - Soak time  
  - The LCD accurately displayed and updated:  
    - Reflow states  
    - Oven/ambient temperature  
    - Running time (both total elapsed time and time at each state) throughout the reflow process based on user input  
  - A Start/Stop button was used for initiating and canceling the reflow process at any time.  

- **Automatic Abort on Error**  
  - The reflow process terminated under the following conditions:  
    - Improper thermocouple placement in the oven.  
    - If the oven failed to reach at least **50℃ within the first 60 seconds** of operation.  

# Design
## Hardware
The hardware consisted of the following components:

| #  | Components               | Quantity |
|----|--------------------------|----------|
| 1  | OP07CDE4                 | 1        |
| 2  | LM335                    | 1        |
| 3  | LMC7660                  | 1        |
| 4  | LM4040                   | 1        |
| 5  | K-type thermocouple       | 1        |
| 6  | 1N4148 diodes             | 6        |
| 7  | LCM-S01602DTR/M          | 1        |
| 8  | FQU13N06LTU              | 1        |
| 9  | CEM-1203(42)             | 1        |
| 10 | FT230XSR                 | 1        |
| 11 | Resistor 240KΩ           | 2        |
| 12 | Resistor 10KΩ            | 1        |
| 13 | Resistor 2KΩ             | 1        |
| 14 | Resistor 1KΩ             | 5        |
| 15 | Resistor 330Ω            | 1        |
| 16 | Push buttons             | 7        |
| 17 | Capacitor 10µF           | 1        |
| 18 | Capacitor 100nF          | 2        |
| 19 | Capacitor 1000nF         | 2        |

I was responsible for designing the hardware and also assisted the firmware group in debugging the finite state machine and display logic. With the OP07 op-amp's maximum gain of 330, the challenge lay in balancing gain and noise. Maximizing gain caused noise amplification, whereas reducing gain made the signal too weak for the microcontroller to read. Selecting resistor values to achieve the desired gain was also challenging. Using a high-value resistor reduced thermal noise but introduced offset error and vice versa. Ultimately, I opted for a gain of 250, using a **240KΩ resistor** and a **1KΩ resistor**.

![Op-amp and resistor configuration](/image/IMG_6DEDEBA132AB-1.jpeg)

The OP07 required a dual power supply of ±5V. The LMC7660 was used to convert the 5V from the microcontroller into ±5V with the help of electrolytic capacitors.

![LMC7660](/image/IMG_50D9E9E52711-1.jpeg)

The system also included an LM335 temperature sensor, which calculated the ambient temperature. A reference voltage of 4.096V, provided by the LM4040, was required. The LM335 was connected to the microcontroller via the bus pins of the LCD.

![LM335](/image/IMG_0AED9FB7C357-1.jpeg)

There were five push buttons used to control the reflow oven. The buttons were connected to the microcontroller via a pull-up resistor. The microcontroller read the state of the buttons and updated the display accordingly. They all shared pin 10 and were connected to the bus pins of the LCD. These buttons controlled the soak temperature, soak time, reflow temperature, reflow time, and start/stop functionality.

![Push_buttons](/image/Screenshot_2025-03-11_at_2.37.59AM.png)

A piezo speaker was connected to an NFET and controlled by the microcontroller. The microcontroller sent signals to turn the NFET on and off, causing the speaker to beep whenever the reflow oven transitioned into a new state.

![Piezospeaker](/image/Screenshot_2025-03-11_at_7.05.34_PM.png)

Finally, the microcontroller generated a PWM signal to drive a solid-state relay, which controlled the toaster oven based on the reflow profile.

![SSR](/image/IMG_6C528E00D089-1.jpeg)

## Software
The software was written in Assembly using the 8051 instruction set. It handled temperature readings from the thermocouple and LM335, updated the display, and controlled the reflow profile. The software was divided into three main parts: the finite state machine, the display logic, and the temperature reading logic.

![Flowchart](/image/Software_highlevel.png)

Since the reflow soldering profile had four states—idle, soak, reflow, and cooldown—the finite state machine transitioned between these states based on elapsed time and oven temperature. The display logic updated the LCD based on the current reflow oven state. The temperature reading logic measured temperatures from the thermocouple and LM335 and updated the display accordingly.

Using Timer 0 and Timer 1, the microcontroller tracked elapsed time and oven temperature. If the oven failed to reach 50℃ within the first 60 seconds, the microcontroller shut off the toaster oven and reset to the idle state.

To create the PWM signal, Timer 2 generated a duty cycle based on the reflow profile (0% for idle, 100% for soak, 20% for reflow, and 0% for cooldown). The PWM signal controlled the solid-state relay, turning the toaster oven on and off.

# Data Collection and Validation
During reflow soldering, the oven temperature was recorded every 0.5 seconds into the terminal and CSV file, in which was plotted in real-time using Python Matplotlib. In the end, our team successfully replicated a reflow soldering profile as close to the ideal temperature plot as possible.

| **Ideal Temperature Profile** | **Actual Temperature Sensor Data** |
|------------------------------|--------------------------------|
|![Ideal Temperature](/image/Ideal_temperature.png) | ![Actual Temperature](/image/Temperature_plot.png) |

# Conclusion and Reflection
This project was a great learning experience in hardware design, firmware development, and real-time data plotting. It was challenging to balance the gain of the OP07 op-amp and to design a reliable finite state machine that transitioned between reflow states accurately. The uses of assembly language and interfacing with hardware components were also new to me and required a lot of debugging, understanding of the fundamentals and attention to detail. Even though our team could not implement original additional features such as adding a soundtrack into the final beep or an automatic oven opening mechanism, we were able to meet all the requirements and create a functional reflow oven.

I was also really proud of the team perserverance and dedication to the project. Despite a difficult start with not everyone in the team understanding the project requirements, we were able to come together, push through till the final hours of the project and delivered an product that was worthy of pads on the back. However, I do wish we had planned the implementation of the additional features better and had more time to test the system.

# Acknowledgements
I want to thank my team members, Jacks Rockford, Matthew Yeun, Harry Nguyen and Adrian Chua for their hard work and dedication to the project. I also want to thank the ELEC 291 instructor, Jesus Calvino-Fraga for his guidance and support throughout the project.

# Project Repository
[View on GitHub](https://github.com/harryHatesCPEN211/ELEC291_project_1)