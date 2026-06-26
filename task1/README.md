The LM5116 was chosen because it can work with a wide range of input voltages and supports our 50 V input requirement. It uses external MOSFETs, so the output current can be increased by selecting suitable MOSFETs. It also has useful features such as current limiting, soft start, and adjustable switching frequency.The duty cycle is calculated to be between 8 to 50% roughly and all the component values are estimated assuming worst-case scenario behaviour. For all passive component calculation, inductor ripple current is estimated to be around 40% of the max current as recommended by the TI datasheet for minimised switching losses and faster load transient response since the inductor size is relatively small.<img width="941" height="330" alt="image" src="https://github.com/user-attachments/assets/54ce333a-b7c8-461b-bf82-825dfea9379f" />
<img width="461" height="567" alt="image" src="https://github.com/user-attachments/assets/e55c0987-23c7-45d3-8995-63d124228b36" />
TIMING RESISTOR: As recommended by TI, the switching frequency is selected to be at a nominal of 250kHz.
<img width="888" height="217" alt="image" src="https://github.com/user-attachments/assets/f28ceb74-6af8-459f-98b1-cddf1c34ac5f" />
<img width="447" height="127" alt="image" src="https://github.com/user-attachments/assets/f5ba807b-3e93-4eda-a463-e3051c48c86f" />
INDUCTOR SELECTION:calculated as per formula given assuming worstcase scenario behaviour(when Vout is 20V).
<img width="862" height="122" alt="image" src="https://github.com/user-attachments/assets/968927df-f56f-4433-bcbf-a3f3054caf4d" />
<img width="420" height="168" alt="image" src="https://github.com/user-attachments/assets/45312ba5-9a8d-465e-bd79-841619a8d621" />
CURRENT SENSE RESISTOR:calculated as per formula given
<img width="915" height="253" alt="image" src="https://github.com/user-attachments/assets/0f53ba06-bf24-4e24-9ae2-d263bbd900e8" />
<img width="466" height="253" alt="image" src="https://github.com/user-attachments/assets/d1579984-13d3-44e3-8394-7e0706943808" />
RAMP CAPACITOR:<img width="870" height="257" alt="image" src="https://github.com/user-attachments/assets/6184968e-19a2-4b65-862d-a5d3c37609d4" />
<img width="401" height="168" alt="image" src="https://github.com/user-attachments/assets/909fa079-d2cb-43d6-9037-28311fa91d84" />
OUTPUT CAPACITOR:Voltage ripple is estimated to be around 1% of the max output voltage. the minimum calculated capacitance is not adequate as it violates the phase margin conditions on the bode plot. WEBENCH suggests a value between 40 to 80 uF, a nominal value of 68uF after checking transient behaviour, current rating and esr was chosen.<img width="970" height="768" alt="image" src="https://github.com/user-attachments/assets/68a0d1a1-3da0-4de3-9201-fea3038878ec" />
<img width="470" height="692" alt="image" src="https://github.com/user-attachments/assets/4d108f8c-e6bb-4262-9c44-d7a5b57af9b2" />as suggested by TI,a polymer hybrid capacitor is used instead of a ceramic capacitor.
INPUT CAPACITOR: by assuming 50% duty cycle as recommended by TI, a standard 4% input ripple voltage is obtained, the net capacitance of the input caps is estimated to 2.3uF.
<img width="863" height="498" alt="image" src="https://github.com/user-attachments/assets/1557e331-3470-4437-b596-4808e5eb93c3" />
<img width="462" height="396" alt="image" src="https://github.com/user-attachments/assets/de4fe1c3-f178-40f8-aef2-b97b63f263d5" />
VCC CAPACITOR: as per datasheet<img width="893" height="162" alt="image" src="https://github.com/user-attachments/assets/5de9c329-3b83-447d-a2c1-2c0932333bf0" />
BOOTSTRAP CAPACITOR: as per datasheet and threshold calculations of gate charge of the chosen mosfet<img width="892" height="358" alt="image" src="https://github.com/user-attachments/assets/38cbcc95-7cad-4471-859e-a989d6579f3a" />
<img width="402" height="207" alt="image" src="https://github.com/user-attachments/assets/f97af5d3-f6b2-43aa-a619-17a5ae2d2dfe" />
SOFT START CAPACITOR:calculations as per datasheet<img width="922" height="273" alt="image" src="https://github.com/user-attachments/assets/5a7e3779-2ea0-4ee0-bb6e-2fb55951626b" />
<img width="425" height="173" alt="image" src="https://github.com/user-attachments/assets/ab372b82-53f7-4244-b775-8e9f4de6f297" />
OUTPUT VOLTAGE DIVIDER: a DAC injector is used to obtain variable voltage, such that when no voltage is injected, the output is set to maximum i.e 20V. refer circuit diagram in attachment.<img width="903" height="216" alt="image" src="https://github.com/user-attachments/assets/0750ae20-d9e6-405c-b91f-5a8f294a3bae" />
<img width="480" height="527" alt="image" src="https://github.com/user-attachments/assets/41187e44-b4b2-4c76-9345-f6928b7eb5fc" />
UVLO DIVIDER: calculation as per datasheet. UV1 is assumed value as per condition given by TI.
<img width="882" height="358" alt="image" src="https://github.com/user-attachments/assets/03ed885e-571a-4cff-8484-264b3728f96b" />
<img width="445" height="252" alt="image" src="https://github.com/user-attachments/assets/2da302cd-2233-4b6c-869f-07732ce25a0b" />
MOSFETS: mosfet of voltage rating of 100V and gate charge of 17nC is chosen so that voltage drop across the bootstrap capacitor is only between 0.1 and 0.2 V
<img width="395" height="93" alt="image" src="https://github.com/user-attachments/assets/31c6ec43-502d-47fd-817f-0aa7b788c9ae" />
COMPENSATION NETWORK: calculations as per datasheet. 3.3nF is assumed value as per the datasheet.<img width="868" height="256" alt="image" src="https://github.com/user-attachments/assets/9dfa68bb-add3-45e6-827b-b64b8fd2515b" />
<img width="851" height="131" alt="image" src="https://github.com/user-attachments/assets/353a4032-24c3-4e98-808a-12eb97e2fd1e" />
<img width="455" height="397" alt="image" src="https://github.com/user-attachments/assets/778d9fe0-0f25-45b2-b99a-5a1d58c56eed" />
<img width="472" height="210" alt="image" src="https://github.com/user-attachments/assets/68de1ba0-2d4f-4c4c-b88f-37d2bea1cc0d" />
SWITCHING DIODE:<img width="467" height="127" alt="image" src="https://github.com/user-attachments/assets/b888ecaf-27dc-43ac-acc4-c50104a5733d" />
FINAL SCHEMATIC:
<img width="1285" height="885" alt="image" src="https://github.com/user-attachments/assets/1d9d1b9a-4aa3-49f9-9e94-de165976e1be" />
PCB DESIGN METHODS: 
The PCB layout was designed based on the LM5116 datasheet and evaluation board recommendations(refer resources). The LM5116 controller was placed close to the MOSFETs, and the bootstrap capacitor was placed close to the HB and SW pins. The input capacitors were placed close to the MOSFETs, while the output capacitors were placed immediately after the inductor. The inductor was placed close to the switching node. VIN, VOUT and PGND were routed using wide copper areas, and the switching node copper area was kept compact. The exposed thermal pad of the LM5116 was connected to the ground plane. FB, COMP and CS signal traces were routed separately from the power stage. Thin traces were used for low-current signal lines connected to the FB, COMP, CS, CSG, RT, SS, EN, HO, LO, HB and DAC feedback network. Gate drive traces between the controller and MOSFETs were kept short. Ground copper pour was used across the PCB.
<img width="757" height="921" alt="image" src="https://github.com/user-attachments/assets/3f619313-09a1-4328-bbec-7bb69fb09aeb" />
the component placement closely follows the layout guidelines and the gerber file reference given in the datasheet and evaluation board file.
<img width="783" height="587" alt="image" src="https://github.com/user-attachments/assets/e94637c6-6210-42df-9e2d-e92514ce68bd" />
<img width="977" height="936" alt="image" src="https://github.com/user-attachments/assets/f3de52c0-61ef-4451-a9a8-7955ecb7cebf" />
















