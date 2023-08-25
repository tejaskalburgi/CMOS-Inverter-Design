# 1 Invertor-Design-and-Analysis

In this project, I'll be working on the design of an inverter and understanding all the parameters involved with it. The design will utilize the models that are present under the skywater 130nm pdk and various open-source tools such as Xschem, NGSPICE, etc.

First, we do the analysis of NMOS and PMOS devices, specifically the 1.8v standard models available inside the pdk. After this, we start with the design of a CMOS inverter that includes a schematic, and measurement of various parameters like noise margin, rise time, fall time, power, etc. 

For the design and simulation of our Inverter, we'll use Xschem for Schematic Capture and Ngspice for Spice Netlist simulation. 

## 1.1 Analysis of MOSFET models
We start with the analysis of MOSFET models present in sky130 pdk. I will use the 1.8v transistor models. Below is the schematic I created in Xschem.

![image](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/9d90ca9f-70ca-4983-a3ee-50507b8027c8)




The components used are:
nfet_01v8.sym - from xschem_sky130 library
vsource.sym - from xschem devices library
code_shown.sym - from xschem devices library

I used the above to plot the basic characteristic plots for an NMOS Transistor, that is, Ids vs Vds and Ids vs Vgs
We can see the DC sweep on the VGS source for different values of VDS:

![2](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/1ff3a14d-5011-4768-b461-4800a3ba72c0)


Similarly, when we sweep the VDS source for different values of VGS, I get the below plot:

![3](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/a9cf558b-f8e1-44d7-9f14-f49f0eb5da42)


## 1.2 CMOS Inverter Design and Analysis
I have designed a Schematic of the Inverter, where the Width ratio of PMOS to NMOS is 2, two and a pulse is applied as input. The schematic is shown below: The transfer characteristics is given below:

![4](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/b9c70aee-dbba-405f-b4b9-201634416bb0)


 
The transfer characteristics for the transient analysis is given below:

![5](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/0411891e-0840-4533-87ea-efbf48cd5222)



Now we want to find the switching threshold at which vin=vout. For this, we need to do the dc analysis of the CMOS inverter. We can convert our inverter into an equivalent symbol as shown below: 

![6](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/5b6e28f6-14ed-4e3e-9f70-ce59525d234e)


The Plot of Vout vs Vin is shown below:

![7](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/237eff7c-1bdc-4335-9337-7db589c1658b)


We can also find lower and higher noise margins. For that, we know that

VOH - Maximum output voltage when it is logic '1'.   
VOL - Minimum output voltage when it is logic '0'.   
VIH - Maximum input voltage that can be interpreted as logic '0'.  
VIL - Minimum input voltage that can be interpreted as logic '1'.  

The noise margins are given as:    
NML(Noise Margin for Low) = VIL - VOL      
NMH(Noise Margin for HIGH) - VOH - VIH     

We can now find the trip point Vm and also the Noise Margins by using the below codes:

![8](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/b1da88b3-2b2a-4026-bca0-a1ff38af7c89)


NML=VIL-VOL  
   =0.7435-0  
   =0.7435   

NMH=VOH-VIH   
   =1.8-0.98   
   =0.82    

The values of VOH and VOL are not exactly 1.8 and 0 respectively, but are somewhat less than 1.8V and greater oV respectively.    

The plot of gain, vout vs vin is shown below:

![9](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/bbdeff71-c542-408d-83a6-2f1874d69cc3)



Now we move forward to calculate propagation delay, rise time and fall time. For this we again move to the transient analysis. 

The schematic as shown below:  

![10](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/11965dfd-8d2f-41bb-8c75-9a68d8ce6029)



We write the following code to find the 50% threshold of vin and vout and then calculate the propagation delay:

![11](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/35d27490-53f4-4f6e-b2b3-58ef70e5259c)


We take the average of tpLH and tpHL to find the overall propagation delay.    

Note that all the measurements are done without considering a load capacitance at the output of the inverter.    
Now we will calculate the rise time of the CMOS inverter. We use the following code to do that:   

![12](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/0752b9fe-72e3-4535-8eba-272a4e067840)


Vout, vin curve with respect to time is shown below:   

![13](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/418fd7b3-1ba4-4402-8fa8-9a97c4b7e527)



Now we consider a load capacitance at the output of our invertor. The value of the capacitance we consider is 0.5pF. The schematic is shown below: 

![14](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/7a2f9635-6f9a-4378-9fb3-bf246151f073)



Note that, for unloaded inverters, doubling the sizes of the NMOS and PMOS doesn't have much effect on rise time. For a loaded inverter, it will have an effect. We will look into how much effect that 
will have by taking the Widths to be 2,1 in the first case and 4,2 in the second case.

For the width of Pmos and NMOS to be 2 and 1, respectively, the rise time in the presence of a load can be calculated as: 

![15](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/a4e6111b-da97-486e-88f1-50c171073ba2)


Vout, vin curve with respect to time is shown below:   

![16](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/32ff03f6-01a3-4f41-adc2-403565baa15e)



For the width of PMOS and NMOS to be 4 and 2, respectively, the rise time in the presence of a load can be calculated as follows: 

![17](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/b651a032-f86d-414b-ab1c-494e76e0b785)



Vout, vin curve with respect to time is shown below:  

![18](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/a4beaae5-04b9-4104-96cb-e1a07e7815c3)


We can see there is a significant drop in the rise time by doubling the widths of NMOS and PMOS. Also, the rise time in the 0.5pF case is more compared to 02pF, as 0.5pF has more charging time
than 0.2pF.

Now we move forward to calculate power in both cases when the load is 0.5pF and when it is 0.2pF. These values will be calculated for widths of PMOS and NMOS as 4 and 2 respectively. 
For 0.5pf case, it is given as : 

![19](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/feea3ca8-2678-421f-9564-f345ce13c5f9)



For 0.2pf case, it is given as : 

![20](https://github.com/KushagraYADAV/Invertor-Design-and-Analysis/assets/65351472/ca06e050-0db8-4867-90b4-d5382eb91934)


We can infer that power is more in the case of 0.5pF case as it draws more current while charging. 























 

