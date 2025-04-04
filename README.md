# Human-Body-Communication
<details>
  <summary><strong>Introduction</strong></summary>
The Wired communication networks and wireless communication technologies like Wi
Fi, Bluetooth, and Infrared communication, each capable of high-speed data transfer 
come with limitations, such as line-of-sight constraints, high power usage, vulnerability 
to signal interception, and lower data throughput. To address these challenges, Human 
body communication (HBC) has emerged as a promising alternative. Computational 
advancements has developed in such a way that energy/bit in computation has become 
less compared to energy/bit for communication of that bit so energy/bit in communication 
became bottleneck for implementing low power embedded systems.  
HBC offers an alternative communication methodology for transferring data. HBC 
eliminates for the need for wires and cables. It is also more secure and efficient compared 
to other wireless communication technologies. As the signals are not radiating in HBC, 
the data signals are not prone to interception. The control of data transfer is also simple as 
the human body is the medium. So, whenever the human body comes in contact with 
both transmitter and receiver modules the data transfer begins and as soon as this 
connection is broken, the data transfer stops.

### Working Principles of HBC 
Human body is good conductor of electricity. Hence the human body can be used as 
conductor to connect two nodes and enable flow of electricity and complete the circuit. 
The communication starts as soon as the path is established between the transmitter and 
receiver through the human body. In normal conditions the human body offers a 
resistance of about 10,000 ohms, but it can be even greater if the person is dehydrated. 
For the safety of the body the current should be limited to 5mA to 9mA. Also the 
maximum voltage that the human body can safely take is 5V. 

### Implementation 
A transmitter is designed which will modulate the message signal with a carrier wave and 
induces displacement current through the human body according to the modulated signal 
variations and when a receiver is in contact with the human body, the circuit is closed and 
the transmission of the signals takes place. The receiver amplifies the received signal and 
is converted back to data using the microcontroller on the receiver end.

</details>

<details>
  <summary><strong>OOK Modulation</strong>
   </summary>
  
## Why Modulation?
Modulating the message signal before transmitting has several advantages:
<br>
1. Reduces intereference while sending the message data over long distance as low frequency signals are more prone to interferences.
<br>

OOK Modulation is a type of AM modulation in which the modulated wave is the product of message and carrier wave. The output is carrier if the input message is bit '1', else zero. Hence, OOK is considered as digital modulation technique.
<br>

![image](https://github.com/user-attachments/assets/9e752135-c999-42b6-9a54-de41d4890d4b)

<br>
Baudrate: 100 bits per second
<br> 
Carrier Frequency: 10 KHz (generated using PWM)
<br>
Two PWMs are used on the transmitter PSoC , one for generating Message signal and the other to generate carrier signal.
The output of these two PWMs are given as input to Mixer so as to obtain the product of message and carrier signals. This product signal represents the OOK modulated signal.
<br>


### OOK Modulation on PSoC:
TopDesign in PSoC <br>

Fc = 1KHz
<br>
Fm = 100Hz
<br>

![image](https://github.com/user-attachments/assets/12017742-960f-48f2-bad1-66a626abaaab)

<br>
Oscilloscope waveform of modulated signal:
<br>
</details>
<details>
  <summary>
   <strong> Modulation and Demodulation on single PSoC</strong> </summary>
  
Fc = 10KHz
<br>
  Fm = 100Hz

  <br>
  
  ![image](https://github.com/user-attachments/assets/53e652ac-559d-41de-a04c-81ea8e901b8e)

<br>

![image](https://github.com/user-attachments/assets/ac033707-4ac3-4059-9f80-5e54bd4da040)

<br>
First Waveform: Modulated signal taken from the ouput of 1st Mixer and given as input to the 2nd Mixer within the same PSoC.
<br>
Second waveform: Demodulated Signal, output of the second Mixer.
<br>
Note: Mixer has been configured as Sample(down) Mixer.
<br>

  ### Conclusion:
  Perfect Demodulation without any loss and the frequency of demodulated signal is matching with the frequency of the message at transmitter.

  ### Mixer Component
PSoC Creator provides a “Mixer” component. It can be used for frequency conversion of an input signal using a local
oscillator (LO) signal as the sampling clock. 
<br>
The Mixer component can be configured in two
configurations:
1. Up Mixer:  Multiplies the input signal with LO.
2. Down Mixer: Operates as a sample and hold circuit on the input signal.

</details>

<details>
  <summary>
<strong>Demodulation on Receiver PSoC with carrier of Transmitter PSoC </strong> </summary>
  
###  Transmitter PSoC:
Both carrier and message signal are generated by PWM with Fc = 10KHz and Fm = 1KHz.
The carrier signal is multiplied with the message signal using the mixer.
<br>

![image](https://github.com/user-attachments/assets/178644f9-2bed-478f-8dc9-07a96e663a3e)

<br>

```
#include "project.h"

int main(void)
{
    CyGlobalIntEnable; /* Enable global interrupts. */

     //Mixer_1_Start();

    for(;;)
    {
        PWM_1_Start();
        PWM_2_Start();
    }
}
```

#### Demodulation
</details>

<details>
  <summary>
    Demodulation on Receiver PSoC
  </summary>
  
 ### PSoC Top design and configuration
   ![image](https://github.com/user-attachments/assets/13210977-104a-4785-b805-65eba7507c51)
   <br>

   The Modulated signal is taken from the transmitter PSoC and given as input to the receiver PSoC. The Mixer in the receiver PSoC is fed with this modulated signal along with the carrier generated by PWM with same frquency as carrier on the transmitter side.
Output Waveform:
<br>
![image](https://github.com/user-attachments/assets/87d8467f-a9ee-4751-92a1-d19a80421d98)
<br>
Note: This is the output when the Mixer is configured as "Up Mixer".
### Inference:
<br>
There has been loss of data after few cycles. This is because of the accumulation of Phase missmatch.
<br>
The carriers generated at the transmitter side and the receiver side doesnot match in terms of phase.
But this phase difference is getting accumulated over time and when the phase difference exceeds T/2, the output of the mixer goes to zero. This is called Phase drift.
 ### Phase Drift
 The unwanted change or deviation in the phase of the signal over time is called phase drift. This can be caused by factors like temperature variations, component aging, or noise. 
 <br>
 <br>
To solve phase drift problems in communication systems, real-time phase drift compensation schemes can be implemented or optimize phase shifts in reconfigurable intelligent surfaces (RIS) can be implemented. 
 
</details>

<details>
  <summary>
  <strong> Quadrature Demodulation </strong>  </summary>

    ### PSoC Top design and configuration
    
  ![image](https://github.com/user-attachments/assets/e48b2d5a-aea4-45f5-8690-b43eab36a785)

</details>

<details>
  <summary>
  Demodulation with higher carrier Frequency </summary>
In this experiment, I have choosen 10KHz as carrier frequency on the transmitter side PSoC and 50KHz square wave is used in the receiver PSoC.
  <br>
  The Mixer at the receiver as two inputs:
  <br>
  1. Modulated wave with Fm = 1KHz and Fc = 10 KHz
  2. Local Carrier wave whose frequency is 50 KHz.
<br>
  ![image](https://github.com/user-attachments/assets/1255b6f3-a630-4ad9-9f99-0de8e7da78bd)

<br>

### Output Waveform:
<br>

![image](https://github.com/user-attachments/assets/8cb10129-98ef-4b1e-aac6-ea75734a6e43)

</details>

### PSoC Implementation of AM Modulation and Demodulation
The message signal and carrier signal are
multiplied by mixer
<br>
The Voltage DAC (VDAC) provides offset to the message signal m(t).
<br>
Modulation index can be defined as the
measure of extent of amplitude variation about a unmodulated carrier. The modulation index is an important factor.
When a level of modulation is too low, the modulation does not utilize the carrier efficiently and if a level of modulation
is too high, the carrier can become over modulated causing sidebands to extend out beyond the allowed bandwidth
causing interference to other users. 
##### Demodulation
![image](https://github.com/user-attachments/assets/a2d95b6f-d763-4f20-966d-aaeeff9fff54)
![image](https://github.com/user-attachments/assets/121c1132-0f3c-4664-b493-1563ec50dae9)
![Uploading image.png…]()

<br>

The AM signal is given to comparator whose reference is AGND. The output of the comparator is square wave with
frequency same as the carrier frequency of AM signal. The output of the comparator is used as a LO signal for the
mixer. The mixer type is set to Down Mixer (or Sample Mixer).The Down Mixer gives a gain close to ‘1’ (when the
signal is sampled at peaks) for the down converted signal. The Down Mixer output has lower harmonic content than
up mixer when the input signal and LO signal have near same frequencies. The Mixer samples the input AM signal at
the rising edges of the LO as shown in following figure.

![image](https://github.com/user-attachments/assets/333da9e0-c562-4847-8686-ce864b89f585)

<br>

The comparator output delay plays
an important role in the demodulation. The ideal delay that gives maximum output is quarter period (90°) of the
carrier. See Figure 18. When the delay is 90°, the mixer samples the AM wave at the peaks. A delay lesser than 90°
still gives a demodulated output; however, the amplitude level is reduced. The comparator typical delay is 90 ns. This
delay makes the mixer sample the AM wave within 45° to 135° from the zero crossing for the frequency range
1.25 MHz to 4 MHz. If the signal frequency is out of this range then, either external delay circuit should be added on
the signal before giving it to ZCD or the signal should be brought within the range before demodulating it.
