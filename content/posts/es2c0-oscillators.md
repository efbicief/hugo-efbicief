---
title: "Oscillators"
date: 2022-06-10T01:15:56+01:00
draft: false
tags: [ "ES2C0", "Electronics", "Oscillators", "Notes" ]
---
# Oscillators
- Consist of an amplifier A, and a frequency selective network β
$$A_{cl}(jω)=\frac{v_o}{v_i}=\frac{A}{1-Aβ(jω)}$$
- For oscillation to occur:
$$Aβ=1\angle 0^{\circ}; |Aβ|=1, : A_{cl}(jω)\rightarrow\infty$$
- 2 aspects of oscillators that we consider:
  - Frequency of oscillation (ω0/f0)
  - Gainloss (amplitude of β); to decide amplifier gain s.t. |Aβ|=1

# Wien bridge
{{<figure src="/wienbridge.png" height=300 title="Wien Bridge Oscillator">}}
- V2 on diagram: use a potential divider s.t.
$$v_2=\frac{Z_2}{Z_1+Z_2}, Z_1=R+\frac{1}{sC}, Z_2=R//\frac{1}{sC}=\frac{R}{1+sCR}$$
letting s=jω. v2 is the transfer function wrt. v1.
- **Frequency of oscillation** in a Wien bridge (frequency selective circuit β):
$$ω_0=\frac{1}{CR}, f_0=\frac{1}{2\pi CR}$$
- **Gainloss**:
$$@ω_0=\frac{1}{CR}, β(jω)=\frac{1}{3};|Aβ|=1\implies A=3$$

# Phase shift oscillator
{{<figure src="/psoscillator.png" height=250 title="Phase Shift Oscillator">}}
- The final opamp is inverting (phase shift=180deg), therefore the 3 RC networks before it must create a 180deg phase shift (60deg each)
- Transfer function of each CR network:
$$\frac{v_o}{v_i}(jω)=\frac{R}{R+\frac{1}{jωC}}=\frac{jωCR}{1+jωCR}$$
- **Frequency of oscillation** in a phase shift oscillator (frequency selective circuit β): s.t. phase shift = 60deg
$$ω_0=\frac{1}{\sqrt{3}CR};\implies\angle60^{\circ}$$
- **Gainloss**:
$$=[@ω_0=\frac{1}{\sqrt{3}CR}]|\frac{v_o}{v_i}|=\frac{1}{2};$$
3 stages means gainloss for whole circuit:
$$=(\frac{1}{2})^3=\frac{1}{8};|Aβ|=1\implies A=-8$$
Gain of final opamp:
$$A=\frac{R_2}{R}=8$$
R not frequency independent for phase shift oscillator; we can change R2 to ensure gain=(-)8.

