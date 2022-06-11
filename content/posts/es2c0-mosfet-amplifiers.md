---
title: "MOSFET Amplifiers"
date: 2022-06-11T03:50:37+01:00
draft: false
tags: [ "ES2C0", "Electronics", "MOSFET", "Notes" ]
---
# BJTs
{{<figure src="/mosfet.png" height=200 title="MOSFET">}}
- MOSFET's drain, gate and source analogous to BJT collector, source and emitter\
MOSFET saturation mode analogous to BJT forward active mode
- k_n = transconduction constant, given to us (defined in terms of semiconductor geometry, etc), V_TN = threshold voltage
- MOSFET is in saturation if the following holds:
$$V_{DS}>V{DS(Sat)}(=V_{GS}-V_{TN})$$
$$V_{GS}>V_{TN}$$
In this case:
$$i_{D}=k_n(V_{GS}-V_{TN})^2$$
- MOSFET is in linear region if the following holds:
$$V_{DS}<V{DS(Sat)}(=V_{GS}-V_{TN})$$

# DC analysis
- Similar circuit to BJTs for biasing

{{<figure src="/mosfet2.png" height=250 title="Voltage divider (Four Resistor) bias circuit (MOSFET)">}}

- Consider:
$$R_{Th}=R_1||R_2, V_{Th}=V_{DD}\frac{R_2}{R_1+R_2}$$
- In MOSFETs:
$$I_G=0\therefore I_D=I_S$$
- Unlike BJTs, V_GS isn't constant:
$$i_D=k_n(V_{GS}-V_{TN})^2\therefore V_{GS}=\sqrt{i_D/k_n}+V_{TN}$$
- When analysing using Thévenin equivalent circuit of above, we can solve for (root) I_D:
$$I_DR_S+\frac{1}{\sqrt{k_n}}\sqrt{I_D}+V_{TN}-V_{Th}=0$$
Quadratic yields 2 values for I_D - need to test which one is correct by applying the criteria for saturation as listed above and checking consistency

# AC analysis
{{<figure src="/mosfetac1.png" height=230 title="Small signal model for a MOSFET.">}}
{{<figure src="/mosfetac2.png" height=230 title="(Including bias resistors)">}}
- **Transconductance** of the MOSFET:
$$g_m=\frac{2I_{DQ}}{V_{GS}-V_{TN}}$$
- Input impedence @ gate
$$R_{ig}=\infty$$
Impedence @ ri
$$R_i=R_{ig}||R_1||R_2=\infty||R_{Th}=R_{Th}$$
Output impedence
$$R_o=R_D$$
- Voltage in
$$V_{in}=V_{gs}+g_mV_{gs}R_S$$
Voltage out
$$V_o=-g_mV_{gs}R_D$$
Therefore: **Voltage gain**:
$$A_v=\frac{V_o}{V_{in}}=\frac{-g_mV_{gs}R_D}{V_{gs}+g_mV_{gs}R_S}=\frac{-g_mR_D}{1+g_mR_S}$$
Assuming g_mR_S >> 1:
$$A_v=\frac{-R_D}{R_S}$$
...just like a BJT!